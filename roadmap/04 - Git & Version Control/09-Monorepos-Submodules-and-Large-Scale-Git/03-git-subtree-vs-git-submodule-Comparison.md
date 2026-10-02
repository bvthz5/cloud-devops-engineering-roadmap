# 03 - git subtree vs. git submodule Comparison

## 1. Why Git Subtree?

`git subtree` merges another repository into your project directly as real committed files and history:
- **Zero Configuration:** No `.gitmodules` file.
- **Seamless Cloning:** Teammates clone the parent repo normally with zero `--recurse-submodules` flags.
- **Bi-directional Sync:** You can edit the subtree files locally, commit them, and push the changes back upstream!

```bash
# Add remote repository as subtree inside vendor/lib
git subtree add --prefix=vendor/lib https://github.com/org/lib.git main --squash

# Pull latest updates from upstream library
git subtree pull --prefix=vendor/lib https://github.com/org/lib.git main --squash

# Push local bug fixes back to the upstream library
git subtree push --prefix=vendor/lib https://github.com/org/lib.git feature-fix
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Submodules](./02-git-submodule-Mechanics-and-Pitfalls.md) | [README](./README.md) | [04 - Sparse Checkout](./04-Sparse-Checkout-and-Monorepo-Scaling.md) |
