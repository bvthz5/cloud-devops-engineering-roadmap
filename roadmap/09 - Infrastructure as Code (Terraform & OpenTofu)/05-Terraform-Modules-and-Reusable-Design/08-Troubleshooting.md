# 08 - Troubleshooting

## Common Module Errors

| Error | Cause | Fix |
|---|---|---|
| `Module not installed` | `terraform init` not run after adding module | Run `terraform init` |
| `Module source has changed` | Source URL modified | Run `terraform init -upgrade` |
| `Error: Unsupported argument` | Variable not defined in module | Add variable to module's `variables.tf` |
| `Module output not found` | Typo in output reference | Check `outputs.tf` in module |
| `Cycle detected` | Circular module dependencies | Restructure to remove circular refs |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
