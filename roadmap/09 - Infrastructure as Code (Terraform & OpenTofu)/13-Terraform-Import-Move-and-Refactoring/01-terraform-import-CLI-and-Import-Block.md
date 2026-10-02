# 01 - terraform import: CLI & Import Block

## 1. CLI Import (Traditional)

```bash
# Import existing VPC
terraform import aws_vpc.main vpc-0abc123def456

# Import resource in a module
terraform import module.networking.aws_vpc.main vpc-0abc123def456
```

**Limitation:** You must manually write the HCL resource block before importing.

## 2. Import Block (Declarative, >= 1.5)

```hcl
import {
  to = aws_vpc.main
  id = "vpc-0abc123def456"
}

import {
  to = aws_subnet.private[0]
  id = "subnet-0abc123"
}
```

## 3. Generate Config from Import (>= 1.5)

```bash
# Auto-generate HCL for imported resources
terraform plan -generate-config-out=generated.tf
```

This creates `generated.tf` with the full resource configuration derived from the cloud API.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (12-Terraform-Provider-Development)](../12-Terraform-Provider-Development/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - moved Block Refactoring →](./02-moved-Block-Refactoring.md) |
