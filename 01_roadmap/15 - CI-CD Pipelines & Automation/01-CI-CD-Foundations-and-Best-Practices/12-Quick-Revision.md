# Quick Revision Notes - CI/CD Foundations & Best Practices

> **Module**: CI/CD Foundations & Best Practices

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

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (02-GitHub-Actions-Workflows-and-Runners) →](../02-GitHub-Actions-Workflows-and-Runners/01-GitHub-Actions-Architecture-Events-Jobs-Steps-and-Runners.md) |
