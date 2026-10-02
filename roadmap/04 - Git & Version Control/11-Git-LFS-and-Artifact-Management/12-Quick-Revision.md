# 12 - Git LFS: Quick Revision Cheat Sheet

## LFS Commands Summary

| Task | Command |
|---|---|
| Initialize LFS | `git lfs install` |
| Track file pattern | `git lfs track "*.ext"` |
| View tracked patterns | `git lfs track` |
| Lock binary file | `git lfs lock <file>` |
| Unlock binary file | `git lfs unlock <file>` |
| View active locks | `git lfs locks` |
| Pull LFS assets manually | `git lfs pull` |
| Clone without LFS binaries | `GIT_LFS_SKIP_SMUDGE=1 git clone <url>` |
| Migrate history to LFS | `git lfs migrate import --include="*.ext"` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Section (05 - Programming & Scripting) →](../../05%20-%20Programming%20%26%20Scripting/01-Bash-Scripting-for-DevOps/01-Bash-Strict-Mode-and-Script-Anatomy.md) |
