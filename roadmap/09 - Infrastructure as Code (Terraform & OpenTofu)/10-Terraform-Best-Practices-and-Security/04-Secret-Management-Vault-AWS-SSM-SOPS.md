# 04 - Secret Management: Vault, AWS SSM & SOPS

## 1. HashiCorp Vault Integration

```hcl
provider "vault" {
  address = "https://vault.example.com"
}

data "vault_generic_secret" "db" {
  path = "secret/data/production/database"
}

resource "aws_db_instance" "main" {
  username = data.vault_generic_secret.db.data["username"]
  password = data.vault_generic_secret.db.data["password"]
}
```

## 2. AWS SSM Parameter Store

```hcl
data "aws_ssm_parameter" "db_password" {
  name            = "/production/database/password"
  with_decryption = true
}

resource "aws_db_instance" "main" {
  password = data.aws_ssm_parameter.db_password.value
}
```

## 3. SOPS (Secrets OPerationS)

```bash
# Encrypt tfvars file with SOPS
sops --encrypt --kms arn:aws:kms:us-east-1:123:key/abc prod.tfvars > prod.enc.tfvars

# Decrypt and use
sops --decrypt prod.enc.tfvars > prod.tfvars
terraform apply -var-file=prod.tfvars
rm prod.tfvars   # Remove decrypted file
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Checkov Policy as Code Scanner](./03-Checkov-Policy-as-Code-Scanner.md) | [Index](../../../README.md) | [05 - Least Privilege IAM for Terraform →](./05-Least-Privilege-IAM-for-Terraform.md) |
