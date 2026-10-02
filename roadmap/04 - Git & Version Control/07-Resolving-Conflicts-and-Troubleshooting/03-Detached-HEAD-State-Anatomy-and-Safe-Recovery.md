# 03 - Detached HEAD State: Anatomy and Safe Recovery

## 1. What is a "Detached HEAD"?

Normally, `HEAD` points to a branch name:
`HEAD ──► refs/heads/main ──► Commit C3`

When you check out an arbitrary commit hash, tag, or remote branch:
```bash
git checkout 4b825dc
```
`HEAD` points **directly to the commit object** rather than a branch pointer!
`HEAD ──► Commit C2`

```
   [ Commit C1 ] ◄──── [ Commit C2 ] ◄──── [ Commit C3 (main) ]
                              ▲
                              │
                             HEAD (Detached!)
```

---

## 2. The Danger & The Fix

If you create commits while in a detached HEAD state, those commits are not associated with any branch. As soon as you run `git checkout main`, **your new commits become orphaned (dangling) and invisible!**

### Safe Recovery: Create a Branch Instantly
```bash
# While still in detached HEAD state, create a branch to capture current commits:
git switch -c new-feature-branch

# Or if you already switched back to main:
# 1. Run git reflog to find the commit SHA you made in detached state
# 2. git branch recovered-work <SHA>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - git rerere](./02-git-rerere-Reuse-Recorded-Resolution.md) | [README](./README.md) | [04 - git reflog](./04-git-reflog-The-Ultimate-DevOps-Safety-Net.md) |
