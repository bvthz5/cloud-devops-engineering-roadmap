# 11 - Git Internals: Self-Assessment MCQs

### Q1. Where are filenames stored in Git's object database?
- A) Inside Blob objects
- B) Inside Tree objects
- C) Inside Commit objects
- D) In the `.git/config` file
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>Tree objects map filenames and modes to Blob and sub-Tree SHA hashes.</details>

---

### Q2. Which low-level command allows inspecting the type and contents of any Git object?
- A) `git inspect-object`
- B) `git cat-file`
- C) `git dump-tree`
- D) `git show-object`
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`git cat-file -t` shows type, and `git cat-file -p` displays contents.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
