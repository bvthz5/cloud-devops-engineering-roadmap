# 03 - Null Resource & Triggers

In Terraform and OpenTofu, infrastructure operations occasionally require running arbitrary orchestration scripts, triggering external APIs, or executing database migrations only when specific configuration files or dependency attributes change. The `null_resource` (and its modern native successor, `terraform_data`) acts as a virtual, non-cloud resource designed specifically to coordinate these actions using **triggers**.

---

## 1. What Is a `null_resource`? (The Legacy Pattern)

Historically, Terraform provided no native way to execute a standalone provisioner without attaching it to a physical cloud resource like an `aws_instance`. If you attached a provisioner directly to a virtual machine, any failure or change would threaten to destroy and recreate the actual production virtual machine!

To solve this coupling problem, HashiCorp created the `hashicorp/null` provider containing a single resource type: `null_resource`. A `null_resource` provisions **zero infrastructure** in the cloud. It exists purely as an entry in `terraform.tfstate` that can host provisioners and re-execute them when specific values change.

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                    NULL_RESOURCE VS DIRECT ATTACHMENT COUPLING                  │
│                                                                                 │
│   ❌ DANGEROUS DIRECT COUPLING                  ✅ DECOUPLED NULL_RESOURCE      │
│                                                                                 │
│   ┌───────────────────────────┐                 ┌───────────────────────────┐   │
│   │    aws_instance.web       │                 │    aws_instance.web       │   │
│   │                           │                 │ (Manages VM in AWS)       │   │
│   │  provisioner "local-exec" │                 └─────────────┬─────────────┘   │
│   │  (Runs DB migration)      │                               │                 │
│   └───────────────────────────┘                 ┌─────────────▼─────────────┐   │
│                 │                               │   null_resource.db_migrate│   │
│         Script Fails!                           │                           │   │
│                 ▼                               │   provisioner "local-exec"│   │
│   Instance marked TAINTED!                      └───────────────────────────┘   │
│   Entire Production VM is                                     │                 │
│   DESTROYED on next apply!                              Script Fails!           │
│                                                               ▼                 │
│                                                 Only null_resource is tainted.  │
│                                                 Production VM remains untouched!│
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Legacy Syntax: `null_resource` with `triggers`

```hcl
terraform {
  required_providers {
    null = {
      source  = "hashicorp/null"
      version = "~> 3.2"
    }
  }
}

resource "aws_db_instance" "postgres" {
  identifier        = "prod-aurora-pg"
  engine            = "postgres"
  instance_class    = "db.t4g.medium"
  allocated_storage = 20
  skip_final_snapshot = true
}

# Standalone execution: Runs Flyway/Alembic database migration
resource "null_resource" "db_migration" {
  # The triggers map dictates WHEN this resource must be destroyed and recreated
  triggers = {
    # Re-run migration whenever the DB endpoint changes
    db_endpoint = aws_db_instance.postgres.endpoint

    # Re-run migration whenever any SQL migration script is modified
    sql_checksum = sha256(join("", [
      for f in fileset("${path.module}/migrations", "*.sql") :
      filesha256("${path.module}/migrations/${f}")
    ]))
  }

  provisioner "local-exec" {
    command = <<EOT
      echo "Executing database migration against ${aws_db_instance.postgres.endpoint}..."
      psql -h ${aws_db_instance.postgres.address} -U dbadmin -d appdb -f ./migrations/schema.sql
    EOT
  }

  depends_on = [aws_db_instance.postgres]
}
```

---

## 2. `terraform_data`: The Modern Built-In Replacement (Terraform >= 1.4 & OpenTofu)

Starting in Terraform 1.4.0 (and inherited by OpenTofu), HashiCorp introduced `terraform_data` as a **built-in core resource**. It completely eliminates the need to declare or download the external `hashicorp/null` provider plugin.

### Advantages of `terraform_data` over `null_resource`
1. **Zero Provider Dependencies:** Built directly into the core engine binary. No provider download during `terraform init`.
2. **Replaces Both `null_resource` and Arbitrary State Storage:** Can store arbitrary typed values in state (`input` argument) and read them back in later plans.
3. **Native Lifecycle Controls:** Uses the standardized `triggers_replace` argument instead of a custom `triggers` map.

```hcl
resource "aws_eks_cluster" "primary" {
  name     = "production-k8s-us-east-1"
  role_arn = aws_iam_role.cluster.arn

  vpc_config {
    subnet_ids = aws_subnet.private[*].id
  }
}

# Modern pattern using terraform_data
resource "terraform_data" "k8s_post_provision" {
  # triggers_replace accepts any list or primitive value.
  # If any element in this list changes, terraform_data is recreated!
  triggers_replace = [
    aws_eks_cluster.primary.certificate_authority[0].data,
    aws_eks_cluster.primary.endpoint,
    filesha256("${path.module}/scripts/bootstrap-cni.sh")
  ]

  provisioner "local-exec" {
    command = "bash ${path.module}/scripts/bootstrap-cni.sh ${aws_eks_cluster.primary.name}"
  }

  depends_on = [aws_eks_cluster.primary]
}
```

---

## 3. How the Trigger Mechanism Evaluates in the Execution Plan

Understanding how triggers work mechanically inside the state reconciliation engine is critical for avoiding unwanted recreation cascades:

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                       TRIGGER RECONCILIATION ENGINE                             │
│                                                                                 │
│   1. Inspect Prior State:               2. Evaluate New Plan:                   │
│      triggers_replace = ["hash_abc123"]    triggers_replace = ["hash_xyz789"]   │
│                                                                                 │
│   3. Compute State Delta:                                                       │
│      Old Hash ("hash_abc123") != New Hash ("hash_xyz789")                       │
│                                                                                 │
│   4. Generate Plan Action:                                                      │
│      ~/# Recreate terraform_data.k8s_post_provision (must be replaced)          │
│                                                                                 │
│   5. Execution Order:                                                           │
│      Step A: Execute Destroy-Time provisioners (if any)                         │
│      Step B: Delete old state entry                                             │
│      Step C: Create new state entry                                             │
│      Step D: Execute Creation-Time provisioners (Running shell script)          │
└─────────────────────────────────────────────────────────────────────────────────┘
```

### Trigger Decision Table

| Trigger Configuration | Trigger Delta Detected? | Plan Action | Execution Behavior |
| :--- | :--- | :--- | :--- |
| `triggers_replace = [var.app_version]` | `var.app_version` updated from `1.0` to `1.1` | **Replace (`-/+`)** | Resource is destroyed and recreated; provisioner runs with new version. |
| `triggers_replace = [filesha256("app.tar.gz")]` | Tarball file content unmodified | **No changes (`<=`)** | Zero action; provisioner is skipped completely (Idempotent). |
| `triggers_replace = [timestamp()]` | Evaluated at runtime | **Replace on EVERY apply** | Runs on every execution; completely breaks idempotency! |
| `triggers_replace = [aws_s3_bucket.data.arn]` | S3 bucket tags updated | **No changes (`<=`)** | Since ARN is unchanged, trigger does not fire despite tag modification. |

---

## 4. Architectural Comparison: `null_resource` vs. `terraform_data`

| Feature | `null_resource` (Legacy) | `terraform_data` (Modern >= 1.4) |
| :--- | :--- | :--- |
| **Provider Requirement** | Requires `hashicorp/null` external plugin | **Built-in core resource** (Zero downloads) |
| **Trigger Argument Name** | `triggers = { ... }` (Must be a map of strings) | `triggers_replace = [...]` (Accepts any type or list) |
| **Value Persistence** | Cannot persist arbitrary data | `input = any` (Persists value into `output` attribute) |
| **State Replacement Support** | Standard resource replacement | Standard resource replacement + dynamic chaining |
| **Recommended Usage** | Deprecated for new configurations | **Industry standard best practice** |

---

## 5. Dangerous Anti-Patterns to Avoid

### ❌ Anti-Pattern 1: The Timestamp Trigger (`timestamp()`)
```hcl
# NEVER DO THIS IN PRODUCTION CI/CD
resource "terraform_data" "broken_deploy" {
  triggers_replace = [timestamp()] # Changes every second!

  provisioner "local-exec" {
    command = "deploy_app.sh"
  }
}
```
**Why this is dangerous:** Running `timestamp()` causes `terraform plan` to **always** show changes (`-/+ replace`). This breaks automated CI/CD pipelines because pull request checks will never report clean, idempotent state.

### ❌ Anti-Pattern 2: Missing Explicit Dependencies (`depends_on`)
If a provisioner script interacts with an AWS resource (e.g., uploading files into an S3 bucket or executing an API against a database), you **must** specify `depends_on = [aws_s3_bucket.target]`. Otherwise, Terraform's Directed Acyclic Graph (DAG) may run the `terraform_data` provisioner in parallel before the target resource finishes creating!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Connection Blocks SSH and WinRM](./02-Connection-Blocks-SSH-and-WinRM.md) | [Index](../../../README.md) | [04 - User Data and Cloud Init vs Provisioners →](./04-User-Data-and-Cloud-Init-vs-Provisioners.md) |

