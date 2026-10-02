# 06 - Supply Chain Security: Provider Signing & Lock Files

## 1. Provider Signing

All official Terraform providers are signed with HashiCorp's GPG key. The lock file records cryptographic hashes to detect tampering.

## 2. Lock File Integrity

```hcl
# .terraform.lock.hcl
provider "registry.terraform.io/hashicorp/aws" {
  version     = "5.31.0"
  constraints = "~> 5.0"
  hashes = [
    "h1:xxxx...",   # ZIP hash
    "zh:yyyy...",   # Source code hash
  ]
}
```

Always commit `.terraform.lock.hcl` to Git. Run `terraform init -upgrade` to update.

## 3. Private Registry for Air-Gapped Environments

```bash
# Mirror providers to local filesystem
terraform providers mirror /path/to/mirror

# Configure CLI to use mirror
cat > ~/.terraformrc <<EOF
provider_installation {
  filesystem_mirror {
    path = "/path/to/mirror"
  }
  direct {}
}
EOF
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Least Privilege IAM for Terraform](./05-Least-Privilege-IAM-for-Terraform.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
