# 05 - Diffing and State Inspection: Working vs. Staged

## 1. The Core Diff Commands

Understanding the reference points of `git diff` prevents accidental commits:

```bash
# 1. Compare Working Directory with Staging Area
# Shows changes made on disk that have NOT been staged yet:
git diff

# 2. Compare Staging Area with the latest commit (HEAD)
# Shows changes that ARE staged and WILL be in the next commit:
git diff --staged
# (or git diff --cached)

# 3. Compare Working Directory directly with latest commit (HEAD)
# Shows all changes (staged AND unstaged) against HEAD:
git diff HEAD

# 4. Compare across two commits or branches
git diff main..feature-branch
```

---

## 2. Advanced Diffing Flags

```bash
# Ignore whitespace differences (indents, tabs, Windows CRLF)
git diff -w

# Display word-by-word differences inline instead of line deletions
git diff --word-diff

# Show summary of files changed and lines added/deleted
git diff --stat
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Basic Workflow Staging Committing and Status](./04-Basic-Workflow-Staging-Committing-and-Status.md) | [Index](../../../README.md) | [06 - Log Mastery Filtering Formatting and Graphing →](./06-Log-Mastery-Filtering-Formatting-and-Graphing.md) |
