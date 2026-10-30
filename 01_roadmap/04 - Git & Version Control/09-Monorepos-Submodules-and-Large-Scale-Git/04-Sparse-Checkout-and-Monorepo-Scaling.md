# 04 - Sparse Checkout and Monorepo Scaling

## 1. What is Sparse Checkout?

In a 50GB monorepo containing 200 services, a frontend developer only cares about `/frontend/web`.
**`git sparse-checkout`** instructs Git to populate the local filesystem with **only the specific directories you specify**, leaving the rest un-downloaded on disk!

---

## 2. Using Sparse Checkout (Cone Mode)

```bash
# Step 1: Initialize sparse checkout in cone mode (fastest algorithm)
git sparse-checkout init --cone

# Step 2: Define the directories you want to populate
git sparse-checkout set frontend/web libs/ui-components

# Verify: Only frontend/web and libs/ui-components exist on your hard drive!
ls -l

# Add another directory later:
git sparse-checkout add libs/auth
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - git subtree vs git submodule Comparison](./03-git-subtree-vs-git-submodule-Comparison.md) | [Index](../../../README.md) | [05 - Partial Clones Blobless and Treeless Clones →](./05-Partial-Clones-Blobless-and-Treeless-Clones.md) |
