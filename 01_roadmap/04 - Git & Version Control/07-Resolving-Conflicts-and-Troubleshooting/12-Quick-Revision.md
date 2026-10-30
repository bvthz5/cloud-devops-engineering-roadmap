# 12 - Resolving Conflicts: Quick Revision Cheat Sheet

## Troubleshooting Cheat Sheet

| Problem | Solution |
|---|---|
| Stuck in conflict merge | `git merge --abort` |
| Stuck in conflict rebase | `git rebase --abort` |
| Deleted a commit via hard reset | `git reflog` $ightarrow$ `git reset --hard <SHA>` |
| Deleted a branch accidentally | `git reflog` $ightarrow$ `git branch <name> <SHA>` |
| Revert a merge commit | `git revert -m 1 <merge_sha>` |
| Enable auto-conflict reuse | `git config --global rerere.enabled true` |
| Automated bug search | `git bisect run <test_script>` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (08-GitOps-and-Declarative-Infrastructure) →](../08-GitOps-and-Declarative-Infrastructure/01-GitOps-Core-Principles-and-Pull-vs-Push.md) |
