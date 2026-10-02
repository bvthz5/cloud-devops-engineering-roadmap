# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
What is the correct order of variable precedence in Terraform (lowest to highest)?
- [x] A) Default → TF_VAR_ → terraform.tfvars → *.auto.tfvars → -var-file → -var
- [ ] B) -var → -var-file → terraform.tfvars → TF_VAR_ → Default
- [ ] C) TF_VAR_ → Default → terraform.tfvars → -var
- [ ] D) Default → terraform.tfvars → -var → TF_VAR_

<details>
<summary>Explanation</summary>
Terraform processes variables from lowest to highest priority. The -var flag on the command line has the highest precedence and overrides all other sources.
</details>

---

### Question 2
When should you prefer `for_each` over `count`?
- [ ] A) When creating identical copies of a resource
- [x] B) When resources need stable, key-based addressing to avoid index reordering issues
- [ ] C) When the resource count is always fixed
- [ ] D) When using conditional resource creation

<details>
<summary>Explanation</summary>
for_each uses string keys for resource addressing (e.g., aws_instance.app["prod"]), which are stable. With count, removing an item shifts all indices, potentially causing unnecessary resource recreation.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
