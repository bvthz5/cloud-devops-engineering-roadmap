# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
How does Terraform CLI separate state between workspaces?
- [ ] A) Separate S3 buckets per workspace
- [x] B) Separate state files under terraform.tfstate.d/<workspace>/ or via workspace_key_prefix
- [ ] C) Different AWS accounts per workspace
- [ ] D) Separate .tf configuration files per workspace

<details>
<summary>Explanation</summary>
CLI workspaces store state in terraform.tfstate.d/<name>/ for local backends or use workspace_key_prefix for remote backends like S3. The same .tf code is shared across all workspaces.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
