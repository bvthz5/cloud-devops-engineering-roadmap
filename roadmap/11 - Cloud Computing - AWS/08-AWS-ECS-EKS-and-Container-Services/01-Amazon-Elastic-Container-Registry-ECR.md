# 01 - Amazon Elastic Container Registry (ECR)

Managed Docker container registry supporting vulnerability scanning and lifecycle rules.

```bash
# ECR Authentication
aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - ECS Task Definitions & Services](./02-Elastic-Container-Service-ECS-Task-Definitions-and-Services.md) |
