# 02 - tfsec Static Security Scanner

## 1. Installation & Usage

```bash
# Install
brew install tfsec    # macOS
# or
curl -s https://raw.githubusercontent.com/aquasecurity/tfsec/master/scripts/install_linux.sh | bash

# Scan current directory
tfsec .

# JSON output for CI
tfsec . --format json > tfsec-results.json

# Ignore specific rules
tfsec . --exclude aws-s3-enable-versioning
```

## 2. Common Findings

| Rule ID | Finding | Severity |
|---|---|---|
| `aws-s3-enable-bucket-encryption` | S3 bucket without encryption | HIGH |
| `aws-ec2-no-public-ip` | EC2 with public IP | HIGH |
| `aws-iam-no-policy-wildcards` | IAM policy with `*` actions | CRITICAL |
| `aws-vpc-no-public-ingress-sgr` | Security group open to 0.0.0.0/0 | CRITICAL |
| `aws-rds-encrypt-instance-storage` | RDS without encryption at rest | HIGH |

## 3. Inline Suppression

```hcl
resource "aws_s3_bucket" "logs" {
  bucket = "access-logs"
  #tfsec:ignore:aws-s3-enable-versioning -- Log bucket doesn't need versioning
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Security Threat Model](./01-IaC-Security-Threat-Model-and-Attack-Surface.md) | [README](./README.md) | [03 - Checkov](./03-Checkov-Policy-as-Code-Scanner.md) |
