# 10 - Hands-On Practice Labs

## Lab 01: Your First Terraform Project

### Objective
Initialize a Terraform project, create an AWS S3 bucket, and understand the full write → plan → apply → destroy lifecycle.

### Steps:
```bash
# 1. Create project structure
mkdir -p ~/iac-lab/01-first-project && cd ~/iac-lab/01-first-project

# 2. Create main.tf
cat > main.tf <<'EOF'
terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
  required_version = ">= 1.5.0"
}

provider "aws" {
  region = "us-east-1"
}

resource "aws_s3_bucket" "lab_bucket" {
  bucket = "iac-lab-${random_id.suffix.hex}"
  tags = {
    Environment = "lab"
    ManagedBy   = "terraform"
  }
}

resource "random_id" "suffix" {
  byte_length = 4
}

output "bucket_name" {
  value = aws_s3_bucket.lab_bucket.id
}
EOF

# 3. Initialize
terraform init

# 4. Format and validate
terraform fmt
terraform validate

# 5. Plan (preview changes)
terraform plan -out=tfplan

# 6. Apply
terraform apply tfplan

# 7. Verify
aws s3 ls | grep iac-lab

# 8. Destroy
terraform destroy -auto-approve
```

---

## Lab 02: Compare Terraform vs OpenTofu

### Steps:
```bash
# Use the same main.tf from Lab 01
# Install OpenTofu
curl -fsSL https://get.opentofu.org/install-opentofu.sh | sh

# Initialize with OpenTofu
tofu init
tofu plan
tofu apply -auto-approve

# Verify identical behavior
tofu destroy -auto-approve
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview QA](./09-Interview-QA.md) | [README](./README.md) | [11 - MCQ](./11-MCQ.md) |
