# 09 - Interview Questions

### Q1: What problems does Terragrunt solve that Terraform alone cannot?
**Answer:**
1. DRY backend configuration (generate once, inherit everywhere)
2. Cross-module dependency orchestration (automatic apply ordering)
3. Multi-environment management with hierarchical config
4. Bulk operations across multiple modules (run-all)
5. DRY provider configuration generation

---

### Q2: How does Terragrunt handle cross-module dependencies?
**Answer:**
Via `dependency` blocks that reference other module paths. Terragrunt reads the dependency's state file to get outputs, builds a DAG, and executes modules in the correct order. `mock_outputs` allow planning before dependencies are applied.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
