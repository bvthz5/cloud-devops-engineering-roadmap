# 11 - MCQ

### Question 1
What does the `moved` block do?
- [ ] A) Moves a resource to a different cloud region
- [x] B) Renames or relocates a resource in state without destroying it
- [ ] C) Migrates state to a new backend
- [ ] D) Imports an existing cloud resource

<details>
<summary>Explanation</summary>
The moved block tells Terraform that a resource has been renamed or moved to a different module path. It updates the state mapping without destroying and recreating the actual cloud resource.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
