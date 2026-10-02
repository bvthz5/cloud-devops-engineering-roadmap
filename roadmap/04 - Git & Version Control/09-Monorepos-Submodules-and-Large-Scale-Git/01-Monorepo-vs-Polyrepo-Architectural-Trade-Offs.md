# 01 - Monorepo vs. Polyrepo Architectural Trade-Offs

## 1. Architectural Comparison

```
MONOREPO (Google / Meta / Uber):
+-----------------------------------------------------------+
| single-git-repo/                                          |
| ├── services/auth/          ├── libs/database/            |
| ├── services/billing/       ├── libs/logger/              |
| └── frontend/web/           └── infra/terraform/          |
+-----------------------------------------------------------+

POLYREPO (Traditional Microservices):
+--------------------+   +--------------------+   +--------------------+
|  repo-auth-service |   | repo-billing-svc   |   | repo-shared-libs   |
+--------------------+   +--------------------+   +--------------------+
```

| Dimension | Monorepo | Polyrepo |
|---|---|---|
| **Code Sharing** | Immediate (Direct local imports) | Complex (Must publish & bump versioned NPM/Pip packages) |
| **Cross-Service Refactoring** | Single atomic commit updates caller & callee | Multi-repo PR coordination; intermediate breaking states |
| **CI/CD Build Times** | Requires affected-graph tooling (Nx, Bazel, Turborepo) | Fast independent builds per repository |
| **Access Control** | Coarse-grained (Requires CODEOWNERS) | Fine-grained per repository |
| **Git Tooling Limits** | Hits Git scaling limits without Scalar/Sparse checkout | Standard Git works effortlessly |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Submodules](./02-git-submodule-Mechanics-and-Pitfalls.md) |
