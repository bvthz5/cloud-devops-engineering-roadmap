# 06 - Git Scalar and Filesystem Virtualization

## 1. What is Scalar?

Developed by Microsoft to scale the Windows OS repository (over 3.5 million files, 300GB+ size), **Scalar** is now integrated directly into core Git (`git scalar`).

Scalar bundles enterprise optimizations into a single command:
- Enables `fsmonitor` (OS file system watcher daemon; eliminates slow `git status` scans).
- Configures blobless partial clone and sparse-checkout automatically.
- Schedules background maintenance (`git maintenance`) to optimize packfiles and commit graphs without blocking the user.

```bash
# Clone a massive repository using Scalar
scalar clone https://github.com/org/huge-repo.git

# Register existing repository with Scalar
scalar register
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Partial Clones Blobless and Treeless Clones](./05-Partial-Clones-Blobless-and-Treeless-Clones.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
