# 04 - File Locking and Binary Conflict Prevention

## 1. The Impossibility of Merging Binaries

If two designers or developers edit the same `.psd` or `.fbx` 3D model file concurrently, Git **cannot merge them**. Resolving the conflict forces someone to discard hours or days of work.

---

## 2. Git LFS File Locking

Git LFS provides server-side locking to prevent concurrent edits:

```bash
# Lock a binary file before editing
git lfs lock assets/textures/model.psd

# View all active locks across the team
git lfs locks

# If a teammate attempts to push changes to this file, their push is rejected!

# Unlock the file after committing and pushing
git lfs unlock assets/textures/model.psd
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Configuring LFS](./03-Configuring-Git-LFS-and-gitattributes.md) | [README](./README.md) | [05 - Repo Migration](./05-Migrating-Bloated-Repositories-to-Git-LFS.md) |
