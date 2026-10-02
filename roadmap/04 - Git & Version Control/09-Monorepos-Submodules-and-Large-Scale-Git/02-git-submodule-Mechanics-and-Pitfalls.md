# 02 - git submodule Mechanics and Pitfalls

## 1. What is a Git Submodule?

A **Submodule** allows you to keep a Git repository as a subdirectory of another Git repository.
- The parent repository does **not** store the submodule's files.
- It stores only:
  1. The remote repository URL in `.gitmodules`.
  2. A specific, pinned **40-character commit hash** in the tree (special file mode `160000`).

```bash
# Add external repo as a submodule
git submodule add https://github.com/org/shared-utils.git libs/shared-utils
```

---

## 2. The Golden Submodule Command
When cloning a repository that contains submodules:
```bash
# Clone and initialize all nested submodules recursively in 1 command:
git clone --recurse-submodules https://github.com/org/parent-repo.git

# If already cloned without flags:
git submodule update --init --recursive
```

---

## 3. The Dreaded Submodule Detached HEAD Trap
By default, `git submodule update` checks out the pinned commit SHA into a **Detached HEAD** state!
If a developer enters `libs/shared-utils/`, edits code, and commits without switching to a branch, **their work will be wiped out during the next `submodule update`**!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Monorepo vs Polyrepo Architectural Trade Offs](./01-Monorepo-vs-Polyrepo-Architectural-Trade-Offs.md) | [Index](../../../README.md) | [03 - git subtree vs git submodule Comparison →](./03-git-subtree-vs-git-submodule-Comparison.md) |
