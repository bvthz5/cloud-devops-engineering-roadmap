# 11 - Git Architecture: Self-Assessment MCQs

### Q1. Which file stores the state of the Git Staging Area on disk?
- A) `.git/HEAD`
- B) `.git/index`
- C) `.git/config`
- D) `.git/COMMIT_EDITMSG`
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>The Staging Area is represented on disk as the binary file `.git/index`.</details>

---

### Q2. Which command compares your current Staging Area against the latest commit in HEAD?
- A) `git diff`
- B) `git diff --staged`
- C) `git diff HEAD..FETCH_HEAD`
- D) `git status -v`
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`git diff --staged` (synonym for `git diff --cached`) compares staged changes to HEAD.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
