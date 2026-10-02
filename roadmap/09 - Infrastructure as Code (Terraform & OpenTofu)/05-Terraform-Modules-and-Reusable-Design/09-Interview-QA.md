# 09 - Interview Questions & Scenarios

### Q1: What is the difference between a root module and a child module?
**Answer:**
The root module is the top-level directory where `terraform apply` is executed. Child modules are reusable components called via `module` blocks. The root module passes input variables to child modules and receives outputs back.

---

### Q2: How do you version and share Terraform modules across teams?
**Answer:**
Publish modules to the Terraform Registry (public) or a private registry. Use Git tags with semantic versioning (v1.0.0). Consumers pin versions using `version = "~> 1.0"`. For private modules, use Git SSH URLs with `?ref=v1.0.0`.

---

### Q3: How would you test a Terraform module before deploying to production?
**Answer:**
Use `terraform test` for native unit/integration tests (HCL-based, ≥ v1.6). Use Terratest for Go-based integration tests that apply, validate, and destroy real infrastructure. Run `terraform validate` and `tflint` for static analysis. Use `checkov` for security scanning.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
