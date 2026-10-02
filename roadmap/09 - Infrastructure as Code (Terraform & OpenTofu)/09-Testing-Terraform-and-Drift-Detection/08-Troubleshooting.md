# 08 - Troubleshooting

| Issue | Cause | Fix |
|---|---|---|
| `terraform test` not found | Terraform < 1.6 | Upgrade to >= 1.6 |
| Tests pass locally, fail in CI | Different provider credentials | Ensure CI has correct AWS/GCP credentials |
| Drift detection false positives | Provider reads computed defaults | Add `ignore_changes` lifecycle block for computed attributes |
| Conftest policy not matching | Wrong JSON path in Rego | Use `terraform show -json` and inspect structure |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
