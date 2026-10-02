# 05 - Packer Image Baking vs Runtime Provisioning

## 1. Build-Time vs Run-Time

```text
PACKER (Build-Time)                    PROVISIONER (Run-Time)
─────────────────                      ──────────────────────
Build image ONCE with all software  -> Install software on EVERY new instance
Store image in AMI/image registry   -> SSH into each instance individually
Launch instances from pre-baked image -> Wait for installation to complete
Boot time: ~30 seconds              -> Boot + install time: 5-15 minutes
Zero drift (same image everywhere)  -> Risk of version differences
```

## 2. Combined Approach (Best Practice)

```text
Packer bakes:                        Terraform deploys:
  - Base OS hardening                  - Launch from baked AMI
  - Common packages (nginx, etc.)      - user_data for instance-specific config
  - Security agents                    - Environment variables
  - Monitoring agents                  - Service registration
```

## 3. Packer + Terraform Integration

```hcl
# Use Packer-built AMI in Terraform
data "aws_ami" "app" {
  most_recent = true
  owners      = ["self"]
  filter {
    name   = "name"
    values = ["app-server-*"]
  }
  filter {
    name   = "tag:Environment"
    values = [var.environment]
  }
}

resource "aws_instance" "app" {
  ami           = data.aws_ami.app.id    # Pre-baked image
  instance_type = "t3.micro"
  # No provisioners needed!
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - User Data & Cloud-Init](./04-User-Data-and-Cloud-Init-vs-Provisioners.md) | [README](./README.md) | [06 - External Data Source](./06-External-Data-Source-and-Custom-Scripts.md) |
