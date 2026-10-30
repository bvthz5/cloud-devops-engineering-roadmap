# 09 - Interview Questions

### Q1: How do you import existing infrastructure into Terraform without downtime?
**Answer:** Use `import` blocks (>= 1.5) with `terraform plan -generate-config-out` to auto-generate HCL. The import operation only modifies state, not actual resources. After import, `terraform plan` should show "No changes."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
