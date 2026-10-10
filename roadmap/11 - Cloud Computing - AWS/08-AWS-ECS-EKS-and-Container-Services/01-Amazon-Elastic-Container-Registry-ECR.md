# 01 - Amazon Elastic Container Registry (ECR) Architecture

> **Focus:** Managed Docker container registries, image tag immutability, vulnerability scanning (Inspector integration), and automated repository lifecycle policies.

---

## 📦 1. Amazon ECR Architecture

**Amazon ECR** is a fully managed container registry that makes it easy to store, manage, share, and deploy container images and OCI artifacts anywhere:

```text
Developer / CI/CD (Docker Build)
                │
                ▼ (docker push)
┌────────────────────────────────────────────────────────┐
│             Amazon ECR Private Repository              │
│  URI: 123456789012.dkr.ecr.us-east-1.amazonaws.com/api │
│                                                        │
│  • Tag Immutability: Prevents overwriting "v1.2.0"     │
│  • Enhanced Vulnerability Scanning (Amazon Inspector)  │
│  • Encryption: KMS Customer Managed Keys (CMK)         │
│  • Lifecycle Policies: Retain only last 30 images      │
└───────────────────────────┬────────────────────────────┘
                            │ (Pull Image)
             ┌──────────────┴──────────────┐
             ▼                             ▼
   Amazon ECS Fargate Tasks       Amazon EKS Worker Pods
```

---

## 🔒 2. Production Security & Lifecycle Policies

```json
{
  "rules": [
    {
      "rulePriority": 1,
      "description": "Keep only the last 30 production images",
      "selection": {
        "tagStatus": "tagged",
        "tagPrefixList": ["v", "release"],
        "countType": "imageCountMoreThan",
        "countNumber": 30
      },
      "action": { "type": "expire" }
    },
    {
      "rulePriority": 2,
      "description": "Expire untagged images older than 14 days",
      "selection": {
        "tagStatus": "untagged",
        "countType": "sinceImagePushed",
        "countUnit": "days",
        "countNumber": 14
      },
      "action": { "type": "expire" }
    }
  ]
}
```

---

### 💻 AWS CLI ECR Authentication & Push Workflow

```bash
# 1. Authenticate Docker CLI to your Private ECR Registry
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com

# 2. Tag and Push Container Image
docker tag my-api:latest 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-api:v1.0.0
docker push 123456789012.dkr.ecr.us-east-1.amazonaws.com/my-api:v1.0.0
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (07-AWS-RDS-DynamoDB-and-Databases)](../07-AWS-RDS-DynamoDB-and-Databases/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Elastic Container Service ECS Task Definitions and Services →](./02-Elastic-Container-Service-ECS-Task-Definitions-and-Services.md) |
