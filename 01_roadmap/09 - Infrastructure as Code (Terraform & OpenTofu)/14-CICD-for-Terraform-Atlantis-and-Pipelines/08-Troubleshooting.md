# 08 - Troubleshooting

| Issue | Fix |
|---|---|
| OIDC: "Not authorized to perform sts:AssumeRoleWithWebIdentity" | Check trust policy conditions (repo, branch) |
| Plan succeeds but apply fails in CI | Ensure saved plan is passed as artifact between jobs |
| Atlantis webhook not triggering | Verify webhook URL and secret in GitHub settings |
| Concurrent pipeline state lock | Use state locking or serialize pipeline runs |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
