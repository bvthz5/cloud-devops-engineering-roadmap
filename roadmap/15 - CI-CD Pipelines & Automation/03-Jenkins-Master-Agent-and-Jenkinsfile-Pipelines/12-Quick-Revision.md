# Quick Revision Notes - Jenkins Master-Agent & Jenkinsfile Pipelines

> **Module**: Jenkins Master-Agent & Jenkinsfile Pipelines

---

## ⚡ Key Cheat Sheet

| Platform / Tool | Primary Pipeline File | Key Strength |
|---|---|---|
| **GitHub Actions** | `.github/workflows/*.yml` | Native GitHub integration, vast marketplace |
| **GitLab CI** | `.gitlab-ci.yml` | Integrated Auto DevOps, multi-project pipelines |
| **Jenkins** | `Jenkinsfile` | Highly customizable, extensive plugin ecosystem |
| **Kaniko** | Kubernetes Build Pod | Rootless container building without Docker daemon |
| **DORA Metrics** | 4 Core Engineering KPIs | Measures deployment velocity and stability |

---

## 📝 Top 5 Rules to Remember
1. **Store pipeline definitions as code in version control** alongside application source.
2. **Use rootless container builders (Kaniko/Buildah)** for Kubernetes CI agents instead of privileged Docker-in-Docker.
3. **Always implement dependency caching** to minimize build times and save bandwidth.
4. **Position unit tests and security scans early** (Shift-Left) to catch bugs before build stages.
5. **Use OIDC short-lived tokens** instead of permanent static cloud credentials in CI/CD secrets.
