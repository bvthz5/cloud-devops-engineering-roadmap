# 01 - The Large Binary File Problem in Git

## 1. Why Git Struggles with Binaries

Git excels at source code because text lines can be compared, diffed, and packed with **sliding window delta compression**:
- If you edit 2 lines in a 1,000-line code file, Git stores only the few bytes of difference in packfiles.
- **With Binaries (Videos, ZIPs, Tarballs, PyTorch model weights `.pt`):** Even a 1-bit modification alters the entire compressed binary hash. Git cannot calculate deltas!
- Every single revision of a 500 MB model adds **another 500 MB permanently to `.git/objects/`**.
- Within 10 commits, cloning the repo requires downloading 5 Gigabytes of data!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (10-Git-Security-Signing-and-Supply-Chain)](../10-Git-Security-Signing-and-Supply-Chain/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Git LFS Architecture and Pointer Files →](./02-Git-LFS-Architecture-and-Pointer-Files.md) |
