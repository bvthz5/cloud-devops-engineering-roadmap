# 06 - Git LFS vs. Dedicated Artifact Repositories

## 1. Architecture Decision Matrix

| Asset Type | Best Tool | Rationale |
|---|---|---|
| **Design assets, datasets tightly coupled to source** | **Git LFS** | Needs branch versioning alongside code |
| **Compiled Binaries / JARs / Wheels** | **JFrog Artifactory / Nexus** | Packaged build artifacts should not live in source repos |
| **Docker / OCI Container Images** | **AWS ECR / Harbor / Docker Hub** | Designed for image layers, vulnerability scanning |
| **Massive ML Model Weights (100GB+)** | **AWS S3 / Hugging Face Hub** | Exceeds GitHub LFS bandwidth and file size limits (2GB limit) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Repo Migration](./05-Migrating-Bloated-Repositories-to-Git-LFS.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
