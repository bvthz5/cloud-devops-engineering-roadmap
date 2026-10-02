# 05 - Cherry-Picking and Selective Commit Porting

## 1. What is Cherry-Picking?

`git cherry-pick` applies the changes introduced by one or more existing commits from another branch onto your current `HEAD` branch.

```
main:           C1 ──► C2 ──► C3 ──► C4 (Bug fix commit: e8f2a1)
                                      
release-v1.0:   C1 ──► C2 ──► C4' (Cherry-picked e8f2a1)
```

---

## 2. Practical Commands

```bash
# Apply single commit to current branch
git cherry-pick e8f2a1

# Cherry-pick without committing (stages changes in Index for editing)
git cherry-pick -n e8f2a1

# Cherry-pick a contiguous range of commits (from A up to B)
git cherry-pick A..B

# If conflict occurs:
# 1. Resolve conflict in files
# 2. git add <resolved_files>
# 3. git cherry-pick --continue
# Or cancel: git cherry-pick --abort
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Interactive Rebasing](./04-Interactive-Rebasing-Squash-Fixup-and-Edit.md) | [README](./README.md) | [06 - Git Stash](./06-Git-Stash-Deep-Dive-and-Work-in-Progress.md) |
