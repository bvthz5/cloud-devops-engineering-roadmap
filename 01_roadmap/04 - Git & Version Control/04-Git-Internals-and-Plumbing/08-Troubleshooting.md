# 08 - Git Internals: Troubleshooting Guide

## 1. Verifying Database Integrity with `git fsck`

```bash
# Comprehensive scan of entire object database
git fsck --full --unreachable

# Identify dangling commits (commits not pointed to by any branch or tag)
git fsck --lost-found
```
Dangling commits are written to `.git/lost-found/commit/` and can be inspected with `git cat-file -p <SHA>`.

---

## 2. Inspecting Object Contents
```bash
# Find what type of object a SHA is
git cat-file -t <SHA>

# Dump the formatted contents of an object
git cat-file -p <SHA>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
