# 01 - Remotes and Tracking Branches Architecture

## 1. How Remotes Work

A **Remote** is simply a bookmark or alias pointing to a repository URL (e.g. `origin` -> `git@github.com:org/repo.git`).

```
LOCAL REPOSITORY                                     REMOTE (GitHub/GitLab)
================                                     ======================
refs/heads/main (Local branch)
      ▲
      │ git merge / git pull
      ▼
refs/remotes/origin/main (Remote-Tracking) ◄── git fetch ──► refs/heads/main
```

---

## 2. `git fetch` vs. `git pull`

- **`git fetch` (Safe & Non-Destructive):** Downloads new objects and updates remote-tracking references (`refs/remotes/origin/*`). **Does NOT touch your local working tree or local branches!**
- **`git pull` (Automated):** Executes `git fetch` followed immediately by `git merge FETCH_HEAD` (or `git rebase` if configured).

```bash
# Recommended DevOps Standard: Configure pull to always rebase
git config --global pull.rebase true
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Forking vs Shared](./02-Forking-Workflow-vs-Shared-Branch-Model.md) |
