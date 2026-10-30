# 04 - Module Design Patterns & Best Practices

## 1. The 4 Production Module Design Patterns

In enterprise cloud architecture, designing reusable Terraform modules requires balancing flexibility with standardization. Platform engineering teams categorize modules into four distinct architectural patterns:

```text
+---------------------------------------------------------------------------------------------------+
|                        THE 4 ENTERPRISE MODULE DESIGN PATTERNS                                    |
+---------------------------------------------------------------------------------------------------+

 1. THE THIN WRAPPER PATTERN                     2. THE OPINIONATED "GOLDEN PATH" BLUEPRINT
 ┌─────────────────────────────────────────┐     ┌─────────────────────────────────────────┐
 │ Wraps a single resource (e.g. S3 bucket)│     │ Bundles complete infrastructure tier:   │
 │ Enforces corporate compliance:          │     │ - VPC + 3 AZs + NAT Gateways            │
 │ - Auto-enables SSE-KMS encryption       │     │ - VPC Flow Logs + CloudWatch + KMS      │
 │ - Blocks all public access by default   │     │ Enforces organizational security standards!
 └─────────────────────────────────────────┘     └─────────────────────────────────────────┘

 3. THE COMPOSABLE ATOMIC PATTERN                4. THE FACTORY / AGGREGATOR PATTERN
 ┌─────────────────────────────────────────┐     ┌─────────────────────────────────────────┐
 │ Unix philosophy: does ONE thing well    │     │ Takes a map of user specifications and  │
 │ - module.subnet                         │     │ dynamically instantiates entire fleets  │
 │ - module.route_table                    │     │ using "for_each" iteration.             │
 │ Caller wires atomic blocks together.    │     │ Example: IAM Role Factory               │
 └─────────────────────────────────────────┘     └─────────────────────────────────────────┘
```

---

## 2. In-Depth Architectural Analysis of Each Pattern

### Pattern 1: The Thin Compliance Wrapper
Used by Central Security and Cloud Governance teams to wrap native resources and enforce mandatory organizational guardrails:
```hcl
# modules/secure-s3-bucket/main.tf
resource "aws_s3_bucket" "this" {
  bucket = var.bucket_name
}

# HARDCODED SECURITY: Developers calling this module CANNOT disable encryption!
resource "aws_s3_bucket_server_side_encryption_configuration" "this" {
  bucket = aws_s3_bucket.this.id
  rule {
    apply_server_side_encryption_by_default {
      sse_algorithm = "aws:kms"
    }
  }
}

resource "aws_s3_bucket_public_access_block" "this" {
  bucket                  = aws_s3_bucket.this.id
  block_public_acls       = true
  block_public_policy     = true
  ignore_public_acls      = true
  restrict_public_buckets = true
}
```

### Pattern 2: The Opinionated "Golden Path" Blueprint
Used to deliver ready-to-use environments for application development teams. The module makes smart default decisions (e.g. 3 availability zones, VPC Flow Logs enabled, KMS encryption active), hiding 200 lines of plumbing behind 10 clean inputs.

### Pattern 3: The Factory Pattern
Accepts a map of definitions and provisions multiple related resources concurrently:
```hcl
# modules/iam-role-factory/variables.tf
variable "service_roles" {
  type = map(object({
    description       = string
    policy_arns       = list(string)
    trust_service     = string
  }))
}

# main.tf: Instantiates all roles dynamically
resource "aws_iam_role" "factory" {
  for_each = var.service_roles
  name     = each.key
  # ...
}
```

---

## 3. Official Module Naming Conventions

When publishing modules to Git repositories or the Terraform Registry, modules must strictly adhere to the standardized three-part naming schema:

$$\text{Repository Name: } \texttt{terraform-<PROVIDER>-<NAME>}$$

- **`terraform-aws-vpc`**: AWS VPC module.
- **`terraform-google-gke`**: Google Cloud Kubernetes Engine module.
- **`terraform-azurerm-aks`**: Azure Managed Kubernetes module.

### Version Tagging:
Every release must be tagged with a strict Semantic Versioning (`SemVer`) Git tag (`v1.0.0`, `v1.2.3`, `v2.0.0`). Never use floating tags like `latest` or `stable`.

---

## 4. The 6 Deadly Module Anti-Patterns

```text
+---------------------------------------------------------------------------------------------------+
|                        DEADLY MODULE ANTI-PATTERNS AND REMEDIATIONS                               |
+---------------------------------------------------------------------------------------------------+

 1. THE "GOD MODULE" MONOLITH:
    Problem: One massive module that provisions VPC, EKS, RDS, Redis, S3, and CloudFront all in one!
    Consequence: Fragile DAG, slow plan times (15+ mins), and catastrophic blast radius.
    Remediation: Split by domain: networking, database, compute.

 2. HARDCODED VALUES:
    Problem: Hardcoding region = "us-east-1" or account_id = "123456789" inside module code.
    Consequence: The module cannot be reused in another region or by another team.
    Remediation: Always expose external configuration as typed input variables.

 3. EMBEDDED PROVIDER BLOCKS:
    Problem: Defining "provider aws {}" directly inside a child module.
    Consequence: Causes inheritance conflicts, prevents provider aliasing, and blocks module removal.
    Remediation: Define providers EXCLUSIVELY in the Root module.

 4. THE "RUSSIAN DOLL" (OVER-NESTING):
    Problem: Module A calls Module B, which calls Module C, which calls Module D (4+ levels deep).
    Consequence: Impossible to debug plan errors; requires duplicating variables through every layer.
    Remediation: Enforce a strict maximum nesting depth of 2 levels.

 5. FRAGILE LIST ITERATION WITH "count":
    Problem: Using count = length(var.list) inside a shared module.
    Consequence: Removing an item causes downstream caller resources to destroy and recreate.
    Remediation: Always use for_each over sets or maps.

 6. SENSITIVE OUTPUT DISCLOSURE:
    Problem: Forgetting sensitive = true on database passwords and private keys.
    Consequence: Plaintext secrets are leaked into CI/CD terminal build logs.
    Remediation: Mark all credential outputs as sensitive = true.
```

---

## 5. Automated Documentation with `terraform-docs`

In enterprise development, documentation must never be maintained manually by hand because it immediately becomes outdated. 

Production teams use **`terraform-docs`** in pre-commit hooks to automatically parse `variables.tf` and `outputs.tf` and generate beautiful Markdown tables directly inside `README.md`:

```yaml
# File: .terraform-docs.yml
formatter: "markdown table"

header-from: "main.tf"
footer-from: ""

sections:
  show:
    - header
    - requirements
    - providers
    - modules
    - resources
    - inputs
    - outputs

sort:
  enabled: true
  by: name

output:
  file: "README.md"
  mode: inject
  template: |-
    <!-- BEGIN_TF_DOCS -->
    {{ .Content }}
    <!-- END_TF_DOCS -->
```

Executing `terraform-docs markdown table --output-file README.md .` updates the documentation in milliseconds, guaranteeing that inputs, types, defaults, and descriptions match code with 100% fidelity.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Module Sources Registry Git S3 and Local](./03-Module-Sources-Registry-Git-S3-and-Local.md) | [Index](../../../README.md) | [05 - Publishing Modules to Terraform Registry →](./05-Publishing-Modules-to-Terraform-Registry.md) |
