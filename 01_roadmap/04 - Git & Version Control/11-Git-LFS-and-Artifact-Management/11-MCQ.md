# 11 - Git LFS: Self-Assessment MCQs

### Q1. What does Git store inside a commit object when a file is tracked via Git LFS?
- A) A compressed delta binary
- B) A 130-byte text pointer file containing the SHA-256 hash and size
- C) An encrypted symlink
- D) A detached branch reference
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>Git stores only the small text pointer file; the binary payload lives in the LFS store.</details>

---

### Q2. Which environment variable instructs Git to clone without automatically downloading LFS binary assets?
- A) `GIT_SKIP_LFS=1`
- B) `GIT_LFS_SKIP_SMUDGE=1`
- C) `GIT_NO_BLOBS=1`
- D) `LFS_DISABLE_DOWNLOAD=true`
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`GIT_LFS_SKIP_SMUDGE=1` skips binary downloads during clone.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
