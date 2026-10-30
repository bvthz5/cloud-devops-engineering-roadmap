# 10 - Hands-On Practice Labs

## Lab 01: Workspace-Based Multi-Environment

```bash
# Create workspaces
terraform workspace new dev
terraform workspace new staging
terraform workspace new prod

# Apply with workspace-specific variable
terraform workspace select dev
terraform apply -var-file=dev.tfvars

terraform workspace select prod
terraform apply -var-file=prod.tfvars
```

## Lab 02: Directory-Based Multi-Environment

```bash
cd environments/dev
terraform init && terraform apply -var-file=dev.tfvars

cd ../prod
terraform init && terraform apply -var-file=prod.tfvars
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
