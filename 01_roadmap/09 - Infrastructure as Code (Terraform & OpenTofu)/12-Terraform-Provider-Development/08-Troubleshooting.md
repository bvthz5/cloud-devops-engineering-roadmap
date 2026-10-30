# 08 - Troubleshooting

| Issue | Fix |
|---|---|
| Provider crash during plan | Add nil checks on API response fields |
| `rpc error: code = Unavailable` | Ensure provider binary is compiled for correct OS/arch |
| Tests failing: resource not found | Check acceptance test cleanup in defer blocks |
| Import not working | Implement ImportState method on resource |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
