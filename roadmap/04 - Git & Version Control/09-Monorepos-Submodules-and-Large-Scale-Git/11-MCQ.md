# 11 - Large-Scale Git: Self-Assessment MCQs

### Q1. Which clone flag downloads the complete commit history graph without downloading file blobs upfront?
- A) `--depth 1`
- B) `--filter=blob:none`
- C) `--single-branch`
- D) `--sparse`
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`--filter=blob:none` performs a blobless partial clone.</details>

---

### Q2. In the parent Git tree, what special file mode identifies a Git Submodule entry?
- A) `100644`
- B) `100755`
- C) `120000`
- D) `160000`
<details><summary><b>View Answer</b></summary><b>Correct Answer: D</b><br>Git reserves mode `160000` for gitlink / submodule pointers.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
