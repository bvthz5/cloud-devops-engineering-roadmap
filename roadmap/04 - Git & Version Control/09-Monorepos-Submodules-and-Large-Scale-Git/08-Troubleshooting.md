# 08 - Large-Scale Git: Troubleshooting Guide

## 1. Fixing Submodule Desync

```bash
# Force sync remote URLs from .gitmodules to local .git/config
git submodule sync --recursive

# Force checkout of pinned commit hashes
git submodule update --init --recursive --force
```

---

## 2. Resetting Sparse Checkout to Full Repository
```bash
# Disable sparse checkout and restore all directories
git sparse-checkout disable
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
