# 08 - Git LFS: Troubleshooting Guide

## 1. Smudge Filter Failed During Clone

```bash
# Error: Error downloading object: Smudge filter failed
# Fix: Clone repository with LFS smudge disabled, then pull manually
GIT_LFS_SKIP_SMUDGE=1 git clone <url>
cd <repo>
git lfs pull
```

---

## 2. Identifying Untracked Large Files
```bash
# Find files larger than 50MB in working directory not tracked by LFS
find . -size +50M -not -path "*/.git/*"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
