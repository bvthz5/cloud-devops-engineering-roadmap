# 06 - run-all & CI/CD Integration

## 1. run-all Commands

```bash
terragrunt run-all init        # Initialize all modules
terragrunt run-all plan        # Plan all modules
terragrunt run-all apply       # Apply all modules (respects dependency order)
terragrunt run-all destroy     # Destroy all modules (reverse dependency order)
terragrunt run-all output      # Show outputs from all modules
```

## 2. CI/CD Pipeline Example (GitHub Actions)

```yaml
name: Terragrunt Deploy
on:
  push:
    branches: [main]
    paths: ['live/prod/**']

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: hashicorp/setup-terraform@v3
      - name: Install Terragrunt
        run: |
          curl -sL https://github.com/gruntwork-io/terragrunt/releases/latest/download/terragrunt_linux_amd64 -o /usr/local/bin/terragrunt
          chmod +x /usr/local/bin/terragrunt
      - name: Plan
        run: cd live/prod && terragrunt run-all plan --terragrunt-non-interactive
      - name: Apply
        run: cd live/prod && terragrunt run-all apply --terragrunt-non-interactive
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Multi Environment with Terragrunt](./05-Multi-Environment-with-Terragrunt.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
