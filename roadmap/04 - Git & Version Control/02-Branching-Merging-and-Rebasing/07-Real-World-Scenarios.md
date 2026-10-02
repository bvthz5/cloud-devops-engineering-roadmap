# 07 - Branching & Merging: Real-World Production Scenarios

## Scenario 1: The Force-Push Disaster on Main

### Incident Summary
An engineer rebased their local branch against `main`, accidentally switched to their local `main` branch, and executed:
```bash
git push --force origin main
```
This rewrote the remote `main` branch history, wiping out 18 production commits merged by other engineers over the preceding 48 hours.

### Resolution
1. An SRE with a recent local checkout ran:
   ```bash
   git reflog show origin/main
   ```
2. Located the SHA of `main` immediately prior to the force push (`b7a4c91`).
3. Restored `main` safely:
   ```bash
   git branch backup-main
   git push --force origin b7a4c91:main
   ```
4. Configured **GitHub / GitLab Branch Protection rules** on `main` to permanently disable force-pushing (`Allow force pushes = Disabled`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Git Stash](./06-Git-Stash-Deep-Dive-and-Work-in-Progress.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
