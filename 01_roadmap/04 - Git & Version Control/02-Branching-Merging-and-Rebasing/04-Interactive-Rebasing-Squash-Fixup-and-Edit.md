# 04 - Interactive Rebasing: Squash, Fixup, and Edit

## 1. Cleaning Up Feature Branches

Before opening a Pull Request, use interactive rebase to turn messy "WIP" and "fix typo" commits into clean, professional commits:

```bash
# Interactively rebase the last 4 commits
git rebase -i HEAD~4
```

An editor opens showing the commits from oldest to newest:

```text
pick 1a2b3c4 feat: implement payment gateway skeleton
s 5d6e7f8 fix typo in database connection
f 9a0b1c2 WIP: remove console.logs
pick 3d4e5f6 test: add unit tests for payment processing
```

---

## 2. Command Directives Table

| Command | Action |
|---|---|
| `pick` (p) | Keep this commit as is. |
| `reword` (r) | Keep commit content, but edit commit message. |
| `edit` (e) | Pause rebase to amend files or split commit. |
| `squash` (s) | Melt commit into the previous commit and **combine commit messages**. |
| `fixup` (f) | Melt commit into the previous commit and **discard this message** (keeps previous message). |
| `drop` (d) | Delete this commit completely. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Git Rebase Mechanics and The Golden Rule](./03-Git-Rebase-Mechanics-and-The-Golden-Rule.md) | [Index](../../../README.md) | [05 - Cherry Picking and Selective Commit Porting →](./05-Cherry-Picking-and-Selective-Commit-Porting.md) |
