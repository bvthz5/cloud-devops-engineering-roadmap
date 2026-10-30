# 04 - git reflog: The Ultimate DevOps Safety Net

## 1. What is the Reflog?

The **Reference Log (Reflog)** records every time the tip of any branch or `HEAD` was updated in your local repository (commits, resets, rebases, merges, checkouts).
- Stored locally in: `.git/logs/HEAD`
- **Never pushed to remote:** It is your private local flight data recorder.

```bash
git reflog
# Output:
# 1a2b3c4 HEAD@{0}: reset: moving to HEAD~1
# 5d6e7f8 HEAD@{1}: commit: add payment processor
# 9a0b1c2 HEAD@{2}: checkout: moving from main to feature
```

---

## 2. Resurrecting Anything with Reflog

### Scenario A: Accidental `git reset --hard` Destroyed Commits
```bash
# You ran 'git reset --hard HEAD~3' and lost 3 commits!
# 1. Run reflog
git reflog
# 2. Identify the commit right before the reset (e.g. HEAD@{1})
# 3. Restore to that exact moment:
git reset --hard HEAD@{1}
```

### Scenario B: Accidentally Deleted a Branch (`git branch -D`)
```bash
git reflog
# Find the commit hash at the tip of the deleted branch (e.g. 5d6e7f8)
git branch feature-recovered 5d6e7f8
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Detached HEAD State Anatomy and Safe Recovery](./03-Detached-HEAD-State-Anatomy-and-Safe-Recovery.md) | [Index](../../../README.md) | [05 - Automated Bug Hunting with git bisect →](./05-Automated-Bug-Hunting-with-git-bisect.md) |
