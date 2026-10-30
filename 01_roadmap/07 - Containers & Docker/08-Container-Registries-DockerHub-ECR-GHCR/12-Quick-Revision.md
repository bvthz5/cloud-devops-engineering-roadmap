# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# AWS ECR Login
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS --password-stdin <account>.dkr.ecr.us-east-1.amazonaws.com

# Cosign Signing
cosign sign --key cosign.key myregistry.com/app:v1.0
cosign verify --key cosign.pub myregistry.com/app:v1.0

# Immutable Pulling
docker pull myregistry.com/app@sha256:<digest>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (09-OCI-Standards-and-Container-Runtimes) →](../09-OCI-Standards-and-Container-Runtimes/01-Open-Container-Initiative-OCI-Specifications.md) |
