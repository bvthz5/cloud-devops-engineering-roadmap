# 08 - Troubleshooting

| Error | Cause | Fix |
|---|---|---|
| `Could not find a terragrunt.hcl file` | Wrong directory | `cd` to directory containing terragrunt.hcl |
| `Cycle detected in dependencies` | Circular dependency blocks | Refactor to break the cycle |
| `Error: mock_outputs not matching` | Mock output type mismatch | Update mock_outputs to match actual output types |
| `run-all failed for module X` | One module in the chain failed | Fix the failing module, then retry run-all |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
