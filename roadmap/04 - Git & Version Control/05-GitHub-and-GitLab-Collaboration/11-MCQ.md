# 11 - GitHub Collaboration: Self-Assessment MCQs

### Q1. What is the key advantage of `git fetch` over `git pull`?
- A) `git fetch` deletes remote branches that were pruned
- B) `git fetch` updates remote tracking references without modifying the local working directory or branches
- C) `git fetch` automatically creates merge commits
- D) `git fetch` pushes local commits upstream
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`git fetch` downloads remote objects and updates `refs/remotes/` safely without touching working files.</details>

---

### Q2. Which flag prevents overwriting a remote branch if another teammate has pushed commits to it?
- A) `--force-all`
- B) `--force-with-lease`
- C) `--safe-push`
- D) `--no-rebase`
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`--force-with-lease` checks remote refs and aborts if unexpected remote commits exist.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
