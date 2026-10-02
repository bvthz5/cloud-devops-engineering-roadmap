# 07 - Git LFS: Real-World Production Scenarios

## Scenario 1: The Surprise Git LFS Bandwidth Outage

### Incident Summary
A team tracked 200MB test video files in Git LFS. Their CI/CD pipeline triggered 150 builds per day across 20 parallel workers. On the 15th of the month, all CI builds failed with:
```
Git LFS: Bandwidth limit exceeded for organization
```

### Root Cause
Every CI runner ran a clean clone, downloading 4GB of LFS assets per build. The team consumed 12 Terabytes of LFS bandwidth in two weeks, blowing past their GitHub plan limits.

### Resolution
1. Configured CI runners to skip LFS download by default:
   ```bash
   export GIT_LFS_SKIP_SMUDGE=1
   git clone <repo>
   ```
2. Pulled LFS assets only in specific integration tests where required:
   ```bash
   git lfs pull --include="fixtures/"
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - LFS vs Artifact Stores](./06-Git-LFS-vs-Dedicated-Artifact-Repositories.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
