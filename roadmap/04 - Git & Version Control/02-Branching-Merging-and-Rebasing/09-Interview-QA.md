# 09 - Branching & Merging: Interview Questions & Answers

### Q1: What is the key architectural difference between `git merge` and `git rebase`?
**Answer:** `git merge` preserves complete chronological history and creates a new 3-way merge commit with two parent pointers connecting the branches. `git rebase` rewires history by replaying each commit from the source branch on top of the target branch tip, creating new commit SHAs and producing a perfectly linear history.

### Q2: What is the Golden Rule of Rebasing?
**Answer:** Never rebase a public or shared branch that other developers have pulled or based work upon. Rebasing rewrites commit hashes; doing so forces teammates into duplicate commits and merge conflicts.

### Q3: How does Git represent a branch internally on disk?
**Answer:** A branch is merely a 41-byte plain-text file in `.git/refs/heads/<branch-name>` containing the 40-character SHA of the latest commit on that branch.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
