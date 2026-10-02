# 04 - RBAC & Governance for IaC

## Governance Framework

| Control | Implementation |
|---|---|
| Who can plan? | CI pipeline (automated) |
| Who can apply to dev? | CI pipeline (automated on merge) |
| Who can apply to prod? | Requires approval from platform team lead |
| Who can modify modules? | CODEOWNERS with required reviews |
| What policies are enforced? | Sentinel/OPA/Checkov in CI |
| How are costs tracked? | Infracost in PR + FinOps tagging |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - State Architecture](./03-State-Architecture-at-Scale.md) | [README](./README.md) | [05 - Performance Optimization](./05-Performance-Optimization-Large-States.md) |
