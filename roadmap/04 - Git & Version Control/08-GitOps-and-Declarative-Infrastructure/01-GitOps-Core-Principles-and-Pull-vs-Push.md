# 01 - GitOps Core Principles and Pull vs. Push

## 1. The 4 Principles of OpenGitOps

1. **Declarative:** The entire desired system state is described declaratively (YAML, JSON, Terraform).
2. **Versioned and Immutable:** The desired state is stored in Git, maintaining complete version history and auditability.
3. **Pulled Automatically:** Software agents in the target environment automatically pull the desired state.
4. **Continuously Reconciled:** Agents continuously compare actual live state with desired state and correct drift.

---

## 2. Push-Based CI/CD vs. Pull-Based GitOps

```
PUSH-BASED (Legacy CI/CD):
[ Git Commit ] ──► [ GitHub Actions Runner ] ──► Direct kubectl / Cloud API ──► [ K8s Cluster ]
* Security Risk: CI runner requires full cluster-admin credentials stored outside the cluster!

PULL-BASED (GitOps):
[ Git Commit ] ──► [ Git Repo (Desired State) ]
                             ▲
                             │ Pulls & Reconciles every 30s
                    +--------------------+
                    | Argo CD Controller | (Runs INSIDE the cluster)
                    +--------------------+
                             │
                             ▼
                    [ Local Kubernetes API ]
* Zero incoming ports open! No credentials leaked to third-party CI runners!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Argo CD & Flux](./02-Argo-CD-and-Flux-CD-Architecture.md) |
