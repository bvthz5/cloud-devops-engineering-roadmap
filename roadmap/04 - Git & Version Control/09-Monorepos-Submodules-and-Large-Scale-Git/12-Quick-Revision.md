# 12 - Large-Scale Git: Quick Revision Cheat Sheet

## Commands Summary

| Task | Command |
|---|---|
| Clone repo with all submodules | `git clone --recurse-submodules <url>` |
| Initialize existing submodules | `git submodule update --init --recursive` |
| Add new submodule | `git submodule add <url> <path>` |
| Pull subtree updates | `git subtree pull --prefix=<path> <url> main --squash` |
| Enable sparse checkout | `git sparse-checkout init --cone` |
| Populate specific folders | `git sparse-checkout set <dir1> <dir2>` |
| Blobless clone for CI | `git clone --filter=blob:none <url>` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Self-Assessment MCQ](./11-MCQ.md) | [README](./README.md) | [10 - Git Security](../10-Git-Security-Signing-and-Supply-Chain/README.md) |
