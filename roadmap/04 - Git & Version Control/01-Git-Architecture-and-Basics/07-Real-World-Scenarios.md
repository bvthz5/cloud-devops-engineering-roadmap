# 07 - Git Architecture: Real-World Production Scenarios

## Scenario 1: The 4GB Local Database Dump Committed to Git

### Incident Summary
A junior backend engineer accidentally ran `git add .` and committed `production_dump.sql` (3.8 GB) along with feature code. Although they realized the mistake and deleted the file in the next commit with `git rm production_dump.sql`, the repository clone size exploded from 20 MB to 4 GB, causing CI/CD runner timeouts.

### Root Cause
Deleting a file in a subsequent commit does **NOT** remove it from Git's historical object database (`.git/objects/pack/`). Every future clone must download all past historical objects.

### Resolution
1. Rewrote repository history to surgically purge the binary blob using `git filter-repo`:
   ```bash
   git filter-repo --path production_dump.sql --invert-paths
   ```
2. Added `*.sql` and `*.dump` to `.gitignore`.
3. Force-pushed the sanitized branch and instructed all engineers to re-clone.

---

## Scenario 2: Uncommitted Production Hotfix Accidental Discard

### Incident Summary
An SRE was debugging an outage on an EC2 instance. They made 50 lines of uncommitted configuration fixes across 3 files. Running `git checkout .` accidentally wiped out all uncommitted disk modifications, requiring 2 hours of reconstruction.

### SRE Lessons Learned
- Uncommitted changes in the Working Directory are **NOT tracked by Git's database**. Once overwritten via `git restore .` or `git checkout .`, they cannot be recovered from `git reflog`.
- Always run `git stash` or create a temporary branch before switching contexts!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Log Mastery](./06-Log-Mastery-Filtering-Formatting-and-Graphing.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
