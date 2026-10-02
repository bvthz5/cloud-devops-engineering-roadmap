# 05 - Environment Promotion Strategies: Kustomize and Helm

## 1. Directory-Based Environment Promotion (Best Practice)

Avoid using long-lived Git branches (`dev`, `staging`, `prod`) for environment promotion. Use **directory paths** with **Kustomize overlays**:

```text
k8s-manifests/
├── base/
│   ├── deployment.yaml
│   └── service.yaml
└── overlays/
    ├── dev/
    │   ├── kustomization.yaml    # Replicas: 1, Dev DB URL
    │   └── patches.yaml
    ├── staging/
    │   └── kustomization.yaml    # Replicas: 2
    └── production/
        └── kustomization.yaml    # Replicas: 10, Prod DB, High Resources
```

### Why Directory-Based Over Branch-Based?
- Environments are visible simultaneously in a single commit.
- Promoted by updating values in Pull Requests rather than merging diverging branch histories.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Secret Management in GitOps SOPS and Vault](./04-Secret-Management-in-GitOps-SOPS-and-Vault.md) | [Index](../../../README.md) | [06 - Drift Detection Self Healing and Rollbacks →](./06-Drift-Detection-Self-Healing-and-Rollbacks.md) |
