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
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [09 - OCI Standards & Container Runtimes](../09-OCI-Standards-and-Container-Runtimes/README.md) |
