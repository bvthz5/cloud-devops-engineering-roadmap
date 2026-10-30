# 09 - Git LFS: Interview Questions & Answers

### Q1: How does Git LFS prevent repository bloat?
**Answer:** Git LFS replaces large binary files inside the Git commit tree with tiny 130-byte text pointer files containing the file's SHA-256 hash and size. The actual large binary payload is stored externally in an object store (e.g. S3). Clones download only the lightweight pointers; binaries are retrieved on-demand.

### Q2: Why is `git lfs lock` necessary when collaborating on binary assets?
**Answer:** Because binary files (such as 3D models, PSDs, or audio files) cannot be merged line-by-line during a merge conflict. File locking allows a developer to lock a binary file on the remote server, preventing teammates from modifying it concurrently until the lock is released.

### Q3: How do you migrate an existing repository that is already bloated with large binary files into Git LFS?
**Answer:** By using `git lfs migrate import --include="*.ext" --everything`. This rewrites past Git commit trees to replace the historical binary blobs with LFS pointer files and uploads the original binaries to the LFS server.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
