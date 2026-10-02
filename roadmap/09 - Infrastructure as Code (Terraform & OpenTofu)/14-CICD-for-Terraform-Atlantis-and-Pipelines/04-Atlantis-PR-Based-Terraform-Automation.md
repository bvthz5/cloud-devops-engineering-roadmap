# 04 - Atlantis: PR-Based Terraform Automation

## 1. What Is Atlantis?

Atlantis is an open-source, self-hosted application that automatically runs `terraform plan` on PRs and allows `terraform apply` via PR comments.

## 2. Workflow

```text
Developer opens PR
    |
    v
Atlantis webhook fires --> terraform plan
    |
    v
Plan output posted as PR comment
    |
    v
Reviewer types: "atlantis apply"
    |
    v
Atlantis runs terraform apply and posts result
```

## 3. atlantis.yaml

```yaml
version: 3
projects:
  - dir: infrastructure/networking
    workspace: default
    autoplan:
      when_modified: ["*.tf", "*.tfvars"]
      enabled: true
  - dir: infrastructure/compute
    workspace: default
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - GitLab CI for Terraform](./03-GitLab-CI-for-Terraform.md) | [Index](../../../README.md) | [05 - Jenkins Terraform Pipeline →](./05-Jenkins-Terraform-Pipeline.md) |
