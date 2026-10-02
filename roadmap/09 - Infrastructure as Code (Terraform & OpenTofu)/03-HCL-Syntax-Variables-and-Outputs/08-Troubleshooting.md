# 08 - Troubleshooting & Diagnostic Runbooks

## Common HCL Errors

| Error | Cause | Fix |
|---|---|---|
| `Invalid value for variable` | Validation rule failed | Check `validation` block conditions |
| `Unsupported attribute` | Wrong attribute name or typo | Check provider documentation |
| `Invalid for_each argument` | Using list instead of set/map | Wrap with `toset()` or use a map |
| `Reference to undeclared variable` | Variable not defined | Add `variable` block or check spelling |
| `Inconsistent conditional result types` | Ternary returns different types | Ensure both branches return same type |
| `Cycle detected in resource dependencies` | Circular references | Break the cycle with `depends_on` or restructure |

## Debug Workflow

```bash
# Enable trace logging
TF_LOG=TRACE terraform plan 2> debug.log

# Search for specific errors
grep -i "error" debug.log | head -20

# Use console for expression testing
terraform console
> length(["a", "b", "c"])
3
> format("web-%s-%02d", "prod", 1)
"web-prod-01"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview QA](./09-Interview-QA.md) |
