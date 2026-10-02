# 02 - Static Analysis: tflint, terraform validate & fmt

## 1. terraform validate & fmt

```bash
terraform fmt -check -recursive    # Check formatting (CI gate)
terraform validate                 # Syntax + internal consistency
```

## 2. tflint

```bash
# Install
curl -s https://raw.githubusercontent.com/terraform-linters/tflint/master/install_linux.sh | bash

# Initialize with AWS ruleset
cat > .tflint.hcl <<EOF
plugin "aws" {
  enabled = true
  version = "0.30.0"
  source  = "github.com/terraform-linters/tflint-ruleset-aws"
}
EOF

tflint --init
tflint --recursive
```

## 3. CI Pipeline Integration

```yaml
# .github/workflows/lint.yml
- name: Format Check
  run: terraform fmt -check -recursive
- name: Validate
  run: terraform init -backend=false && terraform validate
- name: Lint
  run: tflint --recursive
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Testing Pyramid](./01-IaC-Testing-Pyramid-Static-Unit-Integration-E2E.md) | [README](./README.md) | [03 - terraform test](./03-Native-Testing-terraform-test.md) |
