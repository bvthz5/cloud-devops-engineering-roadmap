# 06 - Refactoring Patterns & Best Practices

## 1. Common Refactoring Operations

| Operation | Method |
|---|---|
| Rename resource | `moved` block |
| Extract into module | `moved` block (from root to module path) |
| Split state | `terraform state mv -state-out` |
| Stop managing resource | `removed` block or `terraform state rm` |
| Change resource type | Destroy + recreate (no shortcut) |

## 2. Safe Refactoring Workflow

1. Add `moved` blocks for all renames
2. Run `terraform plan` - should show **zero** destroy/create
3. Apply the refactoring
4. Keep `moved` blocks for 1-2 release cycles
5. Remove `moved` blocks after all team members have applied

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Large Scale Migration Strategies](./05-Large-Scale-Migration-Strategies.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
