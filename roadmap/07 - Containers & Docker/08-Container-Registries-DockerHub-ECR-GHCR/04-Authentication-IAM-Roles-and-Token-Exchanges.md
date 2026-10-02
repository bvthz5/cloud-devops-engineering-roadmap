# 04 - Authentication, IAM, and Token Exchanges

## 1. AWS ECR Authentication Lifecycle

AWS ECR does not use static passwords. You must obtain a temporary 12-hour authorization token using AWS IAM credentials:

```bash
# Retrieve temporary ECR token and authenticate Docker CLI
aws ecr get-login-password --region us-east-1 | \
  docker login --username AWS --password-stdin 123456789012.dkr.ecr.us-east-1.amazonaws.com
```

---

## 2. GitHub Actions Authentication with GHCR

```yaml
steps:
  - name: Log in to GHCR
    uses: docker/login-action@v3
    with:
      registry: ghcr.io
      username: ${{ github.actor }}
      password: ${{ secrets.GITHUB_TOKEN }}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Image Tagging Strategies](./03-Image-Tagging-Strategies-and-Immutability.md) | [README](./README.md) | [05 - Supply Chain Security with Cosign](./05-Supply-Chain-Security-Cosign-and-Image-Signing.md) |
