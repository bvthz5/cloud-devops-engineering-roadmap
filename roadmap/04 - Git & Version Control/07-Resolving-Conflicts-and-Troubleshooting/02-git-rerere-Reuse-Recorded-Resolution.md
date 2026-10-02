# 02 - git rerere: Reuse Recorded Resolution

## 1. Why `git rerere` is a Game Changer

When working on long-lived feature branches or frequently rebasing against `main`, you often resolve the **exact same merge conflict 15 times**.
`rerere` stands for **"Reuse Recorded Resolution"**:
- When enabled, Git takes a fingerprint of any conflict and records your resolution.
- Next time Git encounters that exact same conflict anywhere in the repo, **it automatically resolves it for you!**

```bash
# Enable rerere globally
git config --global rerere.enabled true

# Automatically stage rerere auto-resolutions
git config --global rerere.autoupdate true
```

---

## 2. How it Works Under the Hood
Resolutions are cached in `.git/rr-cache/`. If you rebase a 20-commit branch against `main`, instead of resolving the same conflict at every step, Git applies your cached resolution instantly.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Conflict Anatomy](./01-Merge-Conflict-Anatomy-and-diff3-Visualization.md) | [README](./README.md) | [03 - Detached HEAD](./03-Detached-HEAD-State-Anatomy-and-Safe-Recovery.md) |
