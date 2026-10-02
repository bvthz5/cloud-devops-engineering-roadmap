# 05 - Automated Bug Hunting with git bisect

## 1. Binary Search for Regressions

When a bug is discovered in production, and 500 commits have merged over the past month, manual checkout and testing is impossible.
`git bisect` uses **Binary Search ($O(\log n)$)** to isolate the exact breaking commit:
- 500 commits require only **9 checks**!

---

## 2. Manual Interactive Bisect

```bash
# 1. Start bisect
git bisect start

# 2. Tell Git the current commit is broken
git bisect bad

# 3. Tell Git a known good commit from last month
git bisect good v1.0.0

# Git checks out the middle commit (commit 250).
# Test the app: Does the bug exist?
# If bug exists:
git bisect bad
# If bug does not exist:
git bisect good

# Repeat until Git outputs:
# 4f1a2b3 is the first bad commit!
# Finish and return to main:
git bisect reset
```

---

## 3. Fully Automated Bisect with a Script (`git bisect run`)

Git can run an automated test script autonomously until it locates the culprit:

```bash
git bisect start HEAD v1.0.0
git bisect run pytest tests/test_payment.py
# Git automatically checks out commits, runs the test suite, evaluates exit code 0 vs 1,
# and outputs the exact breaking commit within 60 seconds!
git bisect reset
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - git reflog](./04-git-reflog-The-Ultimate-DevOps-Safety-Net.md) | [README](./README.md) | [06 - Reverting Commits](./06-Reverting-Commits-and-3-Way-Merge-Reversals.md) |
