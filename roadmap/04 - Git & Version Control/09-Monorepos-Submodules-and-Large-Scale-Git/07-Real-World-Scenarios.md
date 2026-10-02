# 07 - Large-Scale Git: Real-World Production Scenarios

## Scenario 1: Submodule Pointer Desynchronization Breaks Production

### Incident Summary
A developer modified a shared utility library inside a submodule, pushed the parent repository's commit, but **forgot to push the submodule's commit to the submodule remote**.
When the production CI/CD runner pulled the parent repository, the build failed with:
```
fatal: reference is not a tree: 9b2a1c0d4e...
Unable to checkout '9b2a1c0d4e' in submodule path 'libs/shared-utils'
```

### Resolution
Enforce push validation in Git configuration:
```bash
# Configure Git to verify that all submodule commits are pushed BEFORE pushing parent:
git config --global push.recurseSubmodules check
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Git Scalar](./06-Git-Scalar-and-Filesystem-Virtualization.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
