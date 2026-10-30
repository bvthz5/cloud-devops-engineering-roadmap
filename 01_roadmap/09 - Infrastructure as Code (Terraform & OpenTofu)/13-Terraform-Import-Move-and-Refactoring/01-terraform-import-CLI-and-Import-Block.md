# 01 - terraform import: CLI & Import Block

> **Concept:** Bringing existing, unmanaged real-world cloud infrastructure (created via AWS Console, ClickOps, or Python scripts) under Terraform management without destroying or recreating anything.

---

## 📜 1. Why Import Exists: The Brownfield Migration Crisis

In enterprise environments, infrastructure rarely starts with a pristine empty cloud account:
- Developers manually click in the AWS/Azure console to build prototypes ("ClickOps").
- Legacy shell scripts or CloudFormation stacks created production VPCs, RDS databases, and Route 53 DNS records.
- **The Problem:** If you simply write `resource "aws_vpc" "main"` and run `terraform apply`, Terraform will attempt to create a **brand new duplicate VPC**, failing with name collisions or wasting thousands of dollars.
- **The Solution:** **`terraform import`** links an existing, active cloud resource ID (e.g. `vpc-0abc123`) to an HCL resource address in your `terraform.tfstate` file!

```text
BEFORE IMPORT (Unmanaged Drift):
[ Real AWS Cloud VPC: vpc-0abc123 ]  <── Isolated, unmanaged by Terraform
               ≠
[ terraform.tfstate: (Empty) ]       <── Terraform has no knowledge of this VPC

AFTER IMPORT (Unified Management):
[ Real AWS Cloud VPC: vpc-0abc123 ]
               ▲
               │ Linked via State Mapping!
               ▼
[ terraform.tfstate: aws_vpc.main.id = "vpc-0abc123" ]
               ▲
               │ Governed by HCL Code
[ main.tf: resource "aws_vpc" "main" { ... } ]
```

---

## ⚡ 2. How Terraform Import Works Internally

```text
┌────────────────────────────────────────────────────────────────────────┐
│                     THE IMPORT WORKFLOW MECHANISM                      │
│                                                                        │
│  1. Identify Target Cloud Resource ID (e.g., "vpc-0987654321fedcba0")  │
│                               │                                        │
│                               ▼                                        │
│  2. Invoke Provider API Read Hook:                                     │
│     Terraform calls aws_vpc.Read() via the AWS Provider plugin.        │
│                               │                                        │
│                               ▼                                        │
│  3. AWS API returns actual live JSON metadata:                         │
│     { "CidrBlock": "10.0.0.0/16", "EnableDnsHostnames": true, ... }    │
│                               │                                        │
│                               ▼                                        │
│  4. State File Injection:                                              │
│     Terraform writes the live attributes into terraform.tfstate.       │
│                               │                                        │
│                               ▼                                        │
│  5. Code Alignment & Plan:                                             │
│     Run 'terraform plan' to verify HCL code matches state with 0 diff! │
└────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 3. Traditional Imperative CLI Import (`terraform import`)

Historically, importing required a manual, imperative terminal command:

```bash
# Syntax: terraform import <RESOURCE_ADDRESS> <CLOUD_RESOURCE_ID>

# Example 1: Import an existing standalone AWS VPC
terraform import aws_vpc.main vpc-0abc123def456

# Example 2: Import an existing Subnet into a named module
terraform import module.networking.aws_subnet.private[0] subnet-0123456789abcdef0

# Example 3: Import an AWS Security Group
terraform import aws_security_group.web_sg sg-0fedcba9876543210
```

### Limitations of the CLI Import Method:
1. **Requires Pre-Existing HCL Code:** In Terraform versions $< 1.5$, you had to write the empty or full `resource` block in your `.tf` file **before** running the command, or it would fail with `Error: resource address not found`.
2. **Not Trackable in Version Control:** Terminal commands are ephemeral. Teammates and CI/CD pipelines cannot code-review a command typed in your local terminal.
3. **Cannot Run in `terraform plan`:** The CLI command immediately modifies `terraform.tfstate` directly without an execution plan confirmation.

---

## 🛡️ 4. Modern Declarative `import {}` Block (Terraform $ge$ 1.5 & OpenTofu)

Starting in **Terraform 1.5** and supported in all versions of **OpenTofu**, import became a first-class, declarative HCL block!

```hcl
# imports.tf

import {
  to = aws_vpc.main
  id = "vpc-0abc123def456"
}

import {
  to = aws_subnet.public_a
  id = "subnet-0123456789abcdef0"
}

import {
  to = module.database.aws_db_instance.postgres
  id = "prod-postgres-cluster-primary"
}
```

### Why the Declarative `import` Block is Superior:
- **Version Controlled:** Written into `.tf` files, committed to Git, and reviewed via Pull Requests.
- **Participates in `terraform plan`:** Shows an explicit plan preview:
  ```text
  Plan: 2 to import, 0 to add, 0 to change, 0 to destroy.
  ```
- **Safe CI/CD Integration:** Automated pipelines can execute imports safely without granting engineers raw terminal access to the production state backend!

---

## 🪄 5. Auto-Generating HCL Code from Import (`--generate-config-out`)

The greatest breakthrough in Terraform 1.5+ is **automated HCL generation**. You no longer need to reverse-engineer cloud attributes by hand!

### Step-by-Step Workflow:

#### Step 1: Write only the `import {}` block in `imports.tf`
```hcl
# imports.tf
import {
  to = aws_s3_bucket.data_lake
  id = "my-company-enterprise-data-lake-prod"
}
```

#### Step 2: Run `terraform plan` with code generation flag:
```bash
terraform plan -generate-config-out=generated_resources.tf
```

#### Step 3: Inspect the generated `generated_resources.tf`:
Terraform queries AWS and automatically writes valid HCL for you!
```hcl
# __generated__ by Terraform
resource "aws_s3_bucket" "data_lake" {
  bucket              = "my-company-enterprise-data-lake-prod"
  force_destroy       = false
  object_lock_enabled = true
  tags = {
    Environment = "Production"
    Team        = "DataEngineering"
  }
}
```

#### Step 4: Review, clean up optional arguments, and apply:
```bash
terraform apply
```
- Terraform imports the resource into `terraform.tfstate`.
- Once imported, you can safely remove the `import {}` block from `imports.tf` (keeping the generated resource code in your repository!).

---

## ⚖️ 6. CLI Import vs. Declarative `import` Block Comparison

| Feature | Traditional CLI (`terraform import`) | Declarative `import {}` Block ($ge$ 1.5) |
| :--- | :--- | :--- |
| **Execution Style** | Imperative shell command. | Declarative HCL code block. |
| **Git & PR Review** | No; executed in engineer's local shell. | **Yes**; committed to Git and reviewed in PRs. |
| **Plan Preview** | None; modifies state directly. | **Yes**; shows `Plan: N to import` in `plan`. |
| **Code Generation** | Not supported. | **Yes** via `-generate-config-out=file.tf`. |
| **Team Safety** | High risk of concurrent state conflicts. | Fully protected by standard state locking. |
| **CI/CD Automation**| Fragile; requires custom bash scripts. | Native; runs cleanly in standard GitHub Actions. |

---

## ⚠️ 7. Common Pitfalls & How to Solve Them

1. **Missing Required Attributes:**
   - Auto-generated code includes read-only parameters or omits custom provider defaults.
   - *Fix:* Always run `terraform plan` after generating config until it reports: `No changes. Your infrastructure matches the configuration.`
2. **State Lock Contention:**
   - Importing while another pipeline is running fails with DynamoDB lock error.
   - *Fix:* Wait for the pipeline or inspect `terraform force-unlock <LOCK-ID>`.
3. **Compound Cloud IDs:**
   - Some resources require multi-part IDs (e.g., AWS Security Group Rule uses `sg-id_type_protocol_from_to_cidr`).
   - *Fix:* Check official Terraform Registry documentation under the **"Import"** section at the bottom of the resource page.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (12-Terraform-Provider-Development)](../12-Terraform-Provider-Development/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - moved Block Refactoring →](./02-moved-Block-Refactoring.md) |
