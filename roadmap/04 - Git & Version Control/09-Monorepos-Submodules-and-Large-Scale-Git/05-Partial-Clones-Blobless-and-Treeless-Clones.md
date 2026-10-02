# 05 - Partial Clones: Blobless and Treeless Clones

## 1. The Problem with Shallow Clones (`--depth 1`)

DevOps engineers frequently use shallow clones (`git clone --depth 1`) in CI runners.
**The downside:** Shallow clones truncate history. You cannot merge, rebase, or run `git describe` for semantic versioning without fetching the missing history (`--unshallow`).

---

## 2. Blobless Clones: The Ultimate CI Optimization (`--filter=blob:none`)

A **Blobless Clone** downloads the complete commit graph and all tree objects, but **zero file contents (blobs)**!
- Download size is **reduced by 80% to 95%**!
- Git only downloads the file blobs when you actually check out a commit.
- History is complete: You can run `git log`, `git checkout`, `git merge`, and `git blame` without errors.

```bash
# The Gold Standard for CI/CD runners:
git clone --filter=blob:none https://github.com/org/massive-repo.git
```

---

## 3. Treeless Clones (`--filter=tree:0`)
Downloads commits only. Trees and blobs are fetched on-demand. Ideal for single-run build containers that only execute `make test` on a single commit.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Sparse Checkout](./04-Sparse-Checkout-and-Monorepo-Scaling.md) | [README](./README.md) | [06 - Git Scalar](./06-Git-Scalar-and-Filesystem-Virtualization.md) |
