# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
What does `find_in_parent_folders()` do in Terragrunt?
- [ ] A) Searches for Terraform state files in parent directories
- [x] B) Searches parent directories for a terragrunt.hcl file to include
- [ ] C) Finds all .tf files in parent directories
- [ ] D) Locates the root module for Terraform

<details>
<summary>Explanation</summary>
find_in_parent_folders() traverses up the directory tree looking for the nearest terragrunt.hcl file, enabling hierarchical configuration inheritance.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
