# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
Which provisioner runs commands on the machine executing Terraform, NOT on the target resource?
- [x] A) local-exec
- [ ] B) remote-exec
- [ ] C) file
- [ ] D) cloud-init

<details>
<summary>Explanation</summary>
local-exec runs on the machine where terraform apply is executed (the operator's workstation or CI runner). remote-exec and file operate on the target resource via SSH/WinRM.
</details>

---

### Question 2
What is the recommended modern replacement for null_resource?
- [ ] A) aws_null_instance
- [x] B) terraform_data
- [ ] C) terraform_null
- [ ] D) resource_trigger

<details>
<summary>Explanation</summary>
terraform_data (Terraform >= 1.4) is the built-in replacement for null_resource. It doesn't require an external provider and provides improved semantics with triggers_replace and input/output attributes.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
