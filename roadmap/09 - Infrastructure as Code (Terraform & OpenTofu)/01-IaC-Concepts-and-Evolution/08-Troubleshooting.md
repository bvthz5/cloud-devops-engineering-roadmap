# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: IaC Adoption Issues

```text
[ Problem: IaC not delivering expected value ]
                    │
                    ▼
    Are all resources managed by IaC?
    ├── NO  ──► Audit with cloud provider tools (AWS Config, Azure Policy)
    │           Import unmanaged resources: terraform import <type>.<name> <id>
    └── YES ──► Is there configuration drift?
                ├── YES ──► Run terraform plan regularly (CI cron)
                │           Block console access; enforce IaC-only changes
                └── NO  ──► Is the state file too large?
                            ├── YES ──► Split into smaller root modules
                            │           Use terraform_remote_state for cross-refs
                            └── NO  ──► Are teams blocked on state locking?
                                        ├── YES ──► Use workspaces or separate states
                                        └── NO  ──► Review module structure & CI pipeline
```

## Common Errors and Fixes

| Error | Cause | Fix |
|---|---|---|
| `Error: No configuration files` | Running terraform in wrong directory | `cd` to directory containing `.tf` files |
| `Error: Failed to query available provider packages` | Network issue or registry blocked | Check proxy settings; use provider mirror |
| `Error: Unsupported Terraform Core version` | `.terraform-version` mismatch | Install correct version via `tfenv` |
| `Error acquiring the state lock` | Previous operation crashed | `terraform force-unlock <LOCK_ID>` |
| `Error: Cycle detected` | Circular resource dependencies | Refactor to remove mutual references |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
