# 09 - Resolving Conflicts: Interview Questions & Answers

### Q1: What is `git reflog` and how does it differ from `git log`?
**Answer:** `git log` displays the commit history traversable from the currently checked-out commit along parent pointers. `git reflog` is a local log recording every change to branch tips and `HEAD` (checkouts, rebases, amends, hard resets). Even if a commit is deleted from branch history, its hash remains recorded in the reflog until pruned by garbage collection.

### Q2: What is the purpose of `git rerere`?
**Answer:** `rerere` stands for "Reuse Recorded Resolution". It tracks how you manually resolve merge conflicts. When identical conflict hunks are encountered in subsequent merges or rebases, Git automatically applies your previous resolution.

### Q3: Why does `git revert` on a merge commit require the `-m` flag?
**Answer:** A merge commit has multiple parents (e.g., Parent 1 from `main`, Parent 2 from the feature branch). Git must know which parent lineage to preserve as the main line. Specifying `-m 1` tells Git that changes brought in by Parent 2 should be reversed relative to Parent 1.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
