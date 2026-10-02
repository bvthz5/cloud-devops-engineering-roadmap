# 12 - Branching & Merging: Quick Revision Cheat Sheet

## Branch & Merge Command Matrix

| Task | Command |
|---|---|
| Create & switch to branch | `git switch -c <name>` or `git checkout -b <name>` |
| Fast-forward merge | `git merge <branch>` |
| Force merge commit creation | `git merge --no-ff <branch>` |
| Abort conflicted merge | `git merge --abort` |
| Rebase onto main | `git rebase main` |
| Interactive rebase last N commits | `git rebase -i HEAD~N` |
| Cherry-pick single commit | `git cherry-pick <SHA>` |
| Stash tracked & untracked files | `git stash push -u -m "msg"` |
| Apply and drop last stash | `git stash pop` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (03-Git-Workflows-Trunk-vs-GitFlow) →](../03-Git-Workflows-Trunk-vs-GitFlow/01-GitFlow-Architecture-and-Lifecycle.md) |
