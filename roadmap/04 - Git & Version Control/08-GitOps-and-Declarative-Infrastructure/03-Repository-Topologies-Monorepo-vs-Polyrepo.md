# 03 - Repository Topologies: Monorepo vs. Polyrepo

## 1. Separating App Code from Manifests

> **Golden Rule of GitOps:** Never store Kubernetes deployment manifests inside the same repository as application source code!

### Why?
1. **CI Infinite Loops:** Automated deployment updates (e.g. CI updating image tag `v1.2.3`) trigger another code build if stored in the same repository.
2. **Access Control:** Developers need write access to application code; only CI and Lead SREs should have merge rights to production manifest repositories.

```
[ App Source Repo ] ──► CI Build & Push Docker Image
                                │
                                ▼ Updates Image Tag
                    [ K8s Config Manifest Repo ] ◄── Argo CD pulls and deploys!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Argo CD and Flux CD Architecture](./02-Argo-CD-and-Flux-CD-Architecture.md) | [Index](../../../README.md) | [04 - Secret Management in GitOps SOPS and Vault →](./04-Secret-Management-in-GitOps-SOPS-and-Vault.md) |
