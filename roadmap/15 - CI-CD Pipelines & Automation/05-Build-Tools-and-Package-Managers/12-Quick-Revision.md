# Quick Revision Notes - Build Tools & Package Managers

> **Module**: Build Tools & Package Managers

---

## ⚡ Key Cheat Sheet

| Deployment / Build Topic | Core Function | Key Advantage |
|---|---|---|
| **Multi-Stage Builds** | Separates build tools from runtime binaries | Minimal image size, reduced CVE attack surface |
| **Blue-Green Deployment** | Two identical environments, instant DNS switch | Instant zero-downtime rollback capability |
| **Canary Deployment** | Incremental traffic shifting (5% -> 100%) | Limits blast radius of production bugs |
| **Expand-Contract Schema** | Multi-phase database migration | Prevents DB schema lockouts during rolling deploys |
| **Artifactory / Nexus** | Private OCI & package proxy caching | Insulates builds from public registry outages |

---

## 📝 Top 5 Rules to Remember
1. **Never copy build SDKs or source code into production containers**; use multi-stage builds.
2. **Ensure database migrations are backward-compatible** using the Expand-Contract pattern.
3. **Use Argo Rollouts or Flagger for automated Canary analysis** with Prometheus metric integration.
4. **Proxy public packages (npm, Maven, Docker) through Artifactory/Nexus** to prevent build breaks from external outages.
5. **Always implement non-root container users (`USER 10001`)** inside runtime container images.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (06-Artifact-Repositories-Nexus-Artifactory) →](../06-Artifact-Repositories-Nexus-Artifactory/01-Artifact-Repository-Architecture-Hosted-Proxy-and-Group-Repos.md) |
