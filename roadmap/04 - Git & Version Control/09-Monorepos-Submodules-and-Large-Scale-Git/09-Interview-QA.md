# 09 - Large-Scale Git: Interview Questions & Answers

### Q1: What is the difference between a blobless clone (`--filter=blob:none`) and a shallow clone (`--depth=1`)?
**Answer:** A shallow clone truncates commit history to a specified depth, making rebases, historical merges, and semantic version tagging difficult or impossible. A blobless clone downloads the complete commit history graph and all tree objects, downloading file contents (blobs) on-demand only when checked out, saving 90% bandwidth while retaining full historical operations.

### Q2: What data does a parent repository actually store for a Git submodule?
**Answer:** The parent repository stores only two things:
1. The remote URL and path mapping inside the `.gitmodules` plain text configuration file.
2. A special tree entry with file mode `160000` containing the exact 40-character commit hash to checkout inside the submodule directory.

### Q3: What is Sparse Checkout?
**Answer:** Sparse checkout allows a developer to populate their local working tree with only a designated subset of directories in a repository (e.g. checking out only 1 microservice in a 1,000-service monorepo).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
