# 05 - Performance Optimization for Large States

| Problem | Solution |
|---|---|
| Plan takes 15+ minutes | Split state into smaller modules |
| AWS API rate limiting | Reduce `-parallelism` (default 10 -> 3-5) |
| Provider re-downloads | Set `TF_PLUGIN_CACHE_DIR` |
| State file > 100MB | Aggressive state splitting by domain |
| Frequent state refresh | Use `-refresh=false` where safe |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - RBAC and Governance for IaC](./04-RBAC-and-Governance-for-IaC.md) | [Index](../../../README.md) | [06 - IaC Platform Engineering →](./06-IaC-Platform-Engineering.md) |
