# 02 - Git LFS Architecture and Pointer Files

## 1. How Git LFS Works

Git Large File Storage replaces large binary files inside the Git tree with a tiny **130-byte text pointer file**:

```text
version https://git-lfs.github.com/spec/v1
oid sha256:4d7a214614ab2935c943f9e0ff69d22eadbb8f32b121812f5691a7b9c722a4b4
size 524288000
```

```
DEVELOPER WORKSTATION                             REMOTE SERVERS
====================                             ==============
[ Git Commit ] ──► Stores 130-byte Pointer ────► [ GitHub Repository ]
        │
  (Clean Filter)
        ▼
   Actual 500MB Binary ────────────────────────► [ Git LFS Object Store (S3/GCS) ]
```

- When you commit, Git LFS intercepts the binary file (**Clean Filter**), computes its SHA-256, uploads the binary directly to an object store (S3/GCS), and places the pointer into Git.
- When you checkout, Git LFS reads the pointer (**Smudge Filter**) and downloads the binary on-demand.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Binary Problem](./01-The-Large-Binary-File-Problem-in-Git.md) | [README](./README.md) | [03 - Configuring LFS](./03-Configuring-Git-LFS-and-gitattributes.md) |
