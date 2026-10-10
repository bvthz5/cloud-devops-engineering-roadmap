# 04 - Automated Code Generation from Import (`-generate-config-out`)

Prior to Terraform 1.5, importing existing cloud infrastructure was one of the most frustrating, error-prone tasks in DevOps. Engineers were forced to write hundreds of lines of complex HCL configuration completely by hand *before* running `terraform import`. If a single argument did not match the cloud API's return format, the command failed. 

Starting in **Terraform 1.5.0** (and OpenTofu), the declarative `import` block works in tandem with the **`-generate-config-out`** flag to query the cloud API and automatically generate clean, native HCL code directly into your repository.

---

## 1. The Paradigm Shift: Manual HCL vs. Automated Generation

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                      THE IMPORT EVOLUTION: BEFORE VS AFTER                      │
│                                                                                 │
│   OLD WAY (Pre-1.5): MANUAL GUESSWORK          NEW WAY (1.5+): CODE GENERATION  │
│                                                                                 │
│   1. Inspect Cloud Console                     1. Write minimal import block    │
│      (VPC, 4 subnets, 12 SG rules)                import {                      │
│   2. Hand-craft 250 lines of .tf                  to = aws_vpc.imported         │
│   3. Run `terraform import` CLI                   id = "vpc-0abc123"            │
│   4. Error: Argument 'enable_dns' type         }                                │
│      mismatch! Fix and repeat...               2. Run:                          │
│   5. Finally imported after 4 hours               terraform plan                │
│                                                      -generate-config-out=gen.tf│
│                                                3. Engine inspects API schema    │
│                                                   and writes HCL automatically! │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. The 4-Stage Automated Import & Generation Lifecycle

```text
┌─────────────────────────────────────────────────────────────────────────────────┐
│                     AUTOMATED IMPORT EXECUTION LIFECYCLE                        │
│                                                                                 │
│   STAGE 1: DECLARE      STAGE 2: GENERATE     STAGE 3: SANITIZE     STAGE 4: APPLY   │
│   ┌───────────────┐     ┌───────────────┐     ┌───────────────┐     ┌─────────┐ │
│   │ Define        │────►│ terraform plan│────►│ Human Review: │────►│ terraform │ │
│   │ import {}     │     │ -generate-    │     │ Prune computed│     │ apply   │ │
│   │ in imports.tf │     │ config-out    │     │ attributes &  │     │ (Binds  │ │
│   └───────────────┘     └───────────────┘     │ add variables │     │ State)  │ │
│                                               └───────────────┘     └─────────┘ │
└─────────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Production Walkthrough: Importing an Existing AWS VPC & Subnet

### Stage 1: Declare the Import Blocks (`imports.tf`)
Create a dedicated file `imports.tf` containing the target resource addresses and cloud IDs:

```hcl
# Declare target resources that currently exist ONLY in the AWS console
import {
  to = aws_vpc.core
  id = "vpc-0824b21901a8f9c12"
}

import {
  to = aws_subnet.public_1a
  id = "subnet-099411bc231a44e99"
}
```

---

### Stage 2: Trigger the Code Generation Engine
Run `terraform plan` specifying an output file destination:

```bash
terraform plan -generate-config-out=generated_resources.tf
```

#### What happens under the hood:
1. Terraform connects to the AWS Provider via gRPC.
2. The provider invokes the AWS EC2 `DescribeVpcs` and `DescribeSubnets` API endpoints using the provided IDs.
3. The provider maps the returned JSON schema directly into valid HCL2 syntax.
4. Terraform outputs a summary showing:
   ```text
   Plan: 2 to import, 0 to add, 0 to change, 0 to destroy.
   Generating configuration for:
     - aws_vpc.core
     - aws_subnet.public_1a
   ```

---

### Stage 3: Inspecting & Sanitizing the Generated HCL

The engine generates raw, complete HCL into `generated_resources.tf`. Below is an example of the generated file:

```hcl
# __generated__ by Terraform
# Please review these resources and adapt them to your code standards.

resource "aws_vpc" "core" {
  cidr_block                       = "10.0.0.0/16"
  enable_dns_hostnames             = true
  enable_dns_support               = true
  instance_tenancy                 = "default"
  assign_generated_ipv6_cidr_block = false

  tags = {
    Environment = "production"
    Name        = "core-enterprise-vpc"
    Owner       = "platform-team"
  }

  # ATTRIBUTES TO PRUNE / SANITIZE:
  # The engine might include default or read-only provider arguments.
  # Review tags to ensure consistency with organizational locals!
}

resource "aws_subnet" "public_1a" {
  vpc_id                  = aws_vpc.core.id # Replace hardcoded VPC ID with resource reference!
  cidr_block              = "10.0.1.0/24"
  availability_zone       = "us-east-1a"
  map_public_ip_on_launch = true

  tags = {
    Name = "core-public-subnet-1a"
  }
}
```

#### SRE Sanitization Checklist:
1. **Replace Hardcoded Cloud IDs with References:** In `aws_subnet`, the generated code may hardcode `vpc_id = "vpc-0824b21901a8f9c12"`. Replace this string with a dynamic reference: `vpc_id = aws_vpc.core.id`.
2. **Prune Computed Defaults:** Remove empty maps, null values, or provider defaults that clutter the configuration.
3. **Parameterize with Variables:** Extract CIDR blocks and environment names into `variables.tf`.
4. **Merge Tagging Policies:** Bind standard enterprise tags to `local.common_tags`.

---

### Stage 4: Apply to Seal the State

Once sanitized, execute the plan:

```bash
terraform apply
```

Terraform writes the cloud metadata directly into `terraform.tfstate`. The output confirms that **zero resources were created or destroyed**, and the imported infrastructure is now 100% under version-controlled code management:

```text
Apply complete! Resources: 2 imported, 0 added, 0 changed, 0 destroyed.
```

> [!TIP]
> **Post-Import Hygiene:** Once `terraform apply` succeeds, you can safely remove the `import {}` blocks from `imports.tf` (or delete the file entirely). The state is now synchronized with your permanent HCL resource blocks!

---

## 4. Key Limitations & Troubleshooting

| Edge Case | Problem Description | Recommended SRE Solution |
| :--- | :--- | :--- |
| **Target File Already Exists** | `terraform plan -generate-config-out=file.tf` fails if `file.tf` already exists on disk. | Always provide a new, non-existent filename (e.g., `generated_v1.tf`). |
| **Compound Import IDs** | Resources requiring multiple identifiers (e.g., AWS Route: `rtb-12345_10.0.0.0/16`). | Consult the Terraform Provider documentation under the "Import" section to obtain the exact ID format required. |
| **Deprecated Provider Arguments** | The generator may emit arguments deprecated in newer provider versions. | Run `terraform validate` and update the generated code to use modern replacement attributes. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - removed Block and State Surgery](./03-removed-Block-and-State-Surgery.md) | [Index](../../../README.md) | [05 - Large Scale Migration Strategies →](./05-Large-Scale-Migration-Strategies.md) |

