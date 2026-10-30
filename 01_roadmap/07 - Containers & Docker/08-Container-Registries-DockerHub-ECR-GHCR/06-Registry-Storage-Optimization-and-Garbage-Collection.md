# 06 - Storage Optimization and Garbage Collection

## 1. ECR Lifecycle Policies

To prevent cloud registries from accumulating terabytes of abandoned test images, enforce automated lifecycle retention policies:

```json
{
  "rules": [
    {
      "rulePriority": 1,
      "description": "Expire untagged images older than 7 days",
      "selection": {
        "tagStatus": "untagged",
        "countType": "sinceImagePushed",
        "countUnit": "days",
        "countNumber": 7
      },
      "action": {
        "type": "expire"
      }
    },
    {
      "rulePriority": 2,
      "description": "Keep only the last 30 production releases",
      "selection": {
        "tagStatus": "tagged",
        "tagPrefixList": ["v"],
        "countType": "imageCountMoreThan",
        "countNumber": 30
      },
      "action": {
        "type": "expire"
      }
    }
  ]
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Supply Chain Security Cosign and Image Signing](./05-Supply-Chain-Security-Cosign-and-Image-Signing.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
