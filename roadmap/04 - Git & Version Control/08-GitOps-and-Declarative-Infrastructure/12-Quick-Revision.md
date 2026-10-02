# 12 - GitOps: Quick Revision Cheat Sheet

## GitOps Architecture Summary

| Dimension | Push Model (CI) | Pull Model (GitOps) |
|---|---|---|
| Execution Location | External CI Runner | Inside Target Cluster |
| Firewall Requirement | Inbound 443 open to Cluster API | Zero inbound ports required |
| Drift Management | Manual intervention | Automated self-healing |
| Rollback Execution | Re-run build pipeline | `git revert HEAD` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Self-Assessment MCQ](./11-MCQ.md) | [README](./README.md) | [09 - Monorepos & Large Git](../09-Monorepos-Submodules-and-Large-Scale-Git/README.md) |
