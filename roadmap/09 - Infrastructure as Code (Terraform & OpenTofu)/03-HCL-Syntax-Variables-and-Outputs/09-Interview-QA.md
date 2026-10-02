# 09 - Interview Questions & Scenarios

### Q1: Explain the difference between `count` and `for_each`. When should you use each?
**Answer:**
`count` uses a numeric index and is best for creating N identical resources. `for_each` uses a map or set and creates resources keyed by string. Use `for_each` when resources are distinct (different configs per key) or when resources may be added/removed independently — `count` causes index shifting that triggers unnecessary destroy/recreate.

---

### Q2: What is a dynamic block and when would you use it?
**Answer:**
A dynamic block generates repeated nested blocks (like `ingress`, `tag`, `setting`) programmatically. It's used when the number of nested blocks varies based on input variables, avoiding hardcoded repetition.

---

### Q3: How does Terraform's variable precedence work?
**Answer:**
From lowest to highest: default value → environment variable (TF_VAR_) → terraform.tfvars → *.auto.tfvars → -var-file flag → -var flag. Higher precedence overrides lower.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
