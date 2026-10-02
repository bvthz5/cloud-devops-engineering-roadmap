# 12 - Git Architecture: Quick Revision Cheat Sheet

## Three Trees Operations Matrix

| Goal | Command |
|---|---|
| Move Working Tree $ightarrow$ Staging Area | `git add <file>` |
| Move Staging Area $ightarrow$ Working Tree (Unstage) | `git restore --staged <file>` |
| Discard Working Tree changes | `git restore <file>` |
| Move Staging Area $ightarrow$ Repository (`HEAD`) | `git commit -m "msg"` |
| Undo last commit, keep changes staged | `git reset --soft HEAD~1` |
| Undo last commit, keep changes in working directory | `git reset --mixed HEAD~1` |
| Undo last commit, destroy all changes (DANGER) | `git reset --hard HEAD~1` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Self-Assessment MCQ](./11-MCQ.md) | [README](./README.md) | [02 - Branching & Merging](../02-Branching-Merging-and-Rebasing/README.md) |
