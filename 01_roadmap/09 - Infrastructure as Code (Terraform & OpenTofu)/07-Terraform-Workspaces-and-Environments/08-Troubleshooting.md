# 08 - Troubleshooting

| Error | Cause | Fix |
|---|---|---|
| `Workspace "prod" doesn't exist` | Workspace not created | `terraform workspace new prod` |
| `Can't delete current workspace` | Trying to delete active workspace | Switch to another workspace first |
| Applied to wrong workspace | Forgot to switch | Use CI/CD with workspace baked into pipeline |
| State locked by different workspace | Same backend key used | Use `workspace_key_prefix` in backend config |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
