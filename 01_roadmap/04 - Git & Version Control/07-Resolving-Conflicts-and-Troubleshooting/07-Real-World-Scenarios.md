# 07 - Resolving Conflicts: Real-World Production Scenarios

## Scenario 1: Re-merging a Reverted Feature Branch

### Incident Summary
An engineer merged `feature/billing` into `main`. It caused an unexpected memory leak in production, so the SRE ran `git revert -m 1 <merge-commit>` to quickly restore production. Later, the engineer fixed the memory leak on `feature/billing` and attempted to merge it back into `main`.
**Disaster:** None of the original billing feature code appeared in `main`!

### Root Cause
From Git's perspective, the commits from `feature/billing` were already present in `main`'s ancestry. The previous revert commit explicitly removed the changes. Simply re-merging does not re-add lines that a subsequent commit explicitly deleted!

### Resolution
You must **revert the revert commit first**, and then merge the fixed branch:
```bash
git checkout main
git revert <revert-commit-hash>
git merge feature/billing
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Reverting Commits and 3 Way Merge Reversals](./06-Reverting-Commits-and-3-Way-Merge-Reversals.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
