# 02 - Enterprise Registries: ECR, GHCR, and Harbor

## 1. Enterprise Registry Comparison

- **AWS ECR**: Deep integration with AWS IAM roles, automated KMS encryption at rest, cross-region replication, and native lifecycle policies.
- **GitHub Container Registry (GHCR)**: Integrated directly into GitHub Actions via `${{ secrets.GITHUB_TOKEN }}`, fine-grained repository permissions.
- **Harbor**: The CNCF enterprise open-source on-premises registry. Features built-in vulnerability scanning (Trivy), automated image signing (Cosign/Notary), role-based access control, and tag retention rules.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - OCI Distribution Spec](./01-OCI-Distribution-Spec-and-Registry-HTTP-API.md) | [README](./README.md) | [03 - Image Tagging Strategies](./03-Image-Tagging-Strategies-and-Immutability.md) |
