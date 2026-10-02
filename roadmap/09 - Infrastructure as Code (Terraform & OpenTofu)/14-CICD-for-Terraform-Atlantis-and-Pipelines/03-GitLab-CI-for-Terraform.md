# 03 - GitLab CI for Terraform

```yaml
stages:
  - validate
  - plan
  - apply

validate:
  stage: validate
  script:
    - terraform init -backend=false
    - terraform fmt -check
    - terraform validate

plan:
  stage: plan
  script:
    - terraform init
    - terraform plan -out=tfplan
  artifacts:
    paths: [tfplan]

apply:
  stage: apply
  script:
    - terraform init
    - terraform apply tfplan
  dependencies: [plan]
  when: manual   # Require manual approval
  only: [main]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - GitHub Actions](./02-GitHub-Actions-for-Terraform.md) | [README](./README.md) | [04 - Atlantis](./04-Atlantis-PR-Based-Terraform-Automation.md) |
