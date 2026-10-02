# 11 - Git Hooks: Self-Assessment MCQs

### Q1. Which Git hook is best suited for validating commit message conventions (e.g. Conventional Commits)?
- A) `pre-commit`
- B) `commit-msg`
- C) `post-commit`
- D) `pre-push`
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`commit-msg` receives the commit message file as input and can validate or reject its format.</details>

---

### Q2. How can a developer bypass a failing pre-commit hook during an emergency?
- A) `git commit --skip-all`
- B) `git commit --no-verify`
- C) `git commit --force`
- D) `git commit --bypass`
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`--no-verify` (or `-n`) bypasses `pre-commit` and `commit-msg` hooks.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
