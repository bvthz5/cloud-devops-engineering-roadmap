# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
What does `terraform plan -detailed-exitcode` return when drift is detected?
- [ ] A) Exit code 0
- [ ] B) Exit code 1
- [x] C) Exit code 2
- [ ] D) Exit code 3

<details>
<summary>Explanation</summary>
Exit code 0 = no changes, 1 = error, 2 = changes detected (drift or pending changes). This is essential for automated drift detection in CI pipelines.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
