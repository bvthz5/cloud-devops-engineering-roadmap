# 06 - Git Stash Deep Dive and Work-in-Progress

## 1. Managing Unfinished Work

When you need to switch branches urgently to fix an outage but your current work is half-finished and uncommittable, use `git stash`.

```bash
# Save modified tracked files with a descriptive label
git stash push -m "WIP: redis caching logic"

# Include untracked files in the stash
git stash push -u -m "WIP with new files"

# View list of all saved stashes
git stash list
# stash@{0}: WIP: redis caching logic
# stash@{1}: WIP: user auth refactor
```

---

## 2. Applying and Dropping Stashes

```bash
# Apply most recent stash and remove it from stash list
git stash pop

# Apply stash WITHOUT removing it from stash list
git stash apply stash@{1}

# View changes stored inside a stash
git stash show -p stash@{0}

# Drop specific stash
git stash drop stash@{0}

# Clear all stashes (Caution!)
git stash clear
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Cherry Picking and Selective Commit Porting](./05-Cherry-Picking-and-Selective-Commit-Porting.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
