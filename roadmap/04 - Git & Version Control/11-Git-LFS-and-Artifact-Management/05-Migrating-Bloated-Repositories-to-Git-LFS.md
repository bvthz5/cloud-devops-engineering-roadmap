# 05 - Migrating Bloated Repositories to Git LFS

## 1. Converting Historical Binaries

If a repository is already 15GB because binaries were committed in the past without LFS, running `git lfs track` only affects future commits!
To shrink the repository, you must rewrite history using **`git lfs migrate`**:

```bash
# Analyze historical file extensions consuming the most space
git lfs migrate info

# Rewrite all past commits to convert *.zip and *.tar.gz into LFS pointers:
git lfs migrate import --include="*.zip,*.tar.gz" --everything

# Force push the rewritten history
git push --force --all
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - File Locking](./04-File-Locking-and-Binary-Conflict-Prevention.md) | [README](./README.md) | [06 - LFS vs Artifact Stores](./06-Git-LFS-vs-Dedicated-Artifact-Repositories.md) |
