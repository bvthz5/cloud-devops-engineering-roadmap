# Module 07: Merge Conflicts, Reflog, and Disaster Recovery

Welcome to **Module 07: Resolving Conflicts and Troubleshooting**. Merge conflicts and accidental deletions are inevitable in software engineering. Mastering diagnostic tools, `git rerere`, `git reflog`, and `git bisect` turns high-stress disasters into trivial, automated recoveries.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Deconstruct the anatomy of a **Merge Conflict** and configure **`diff3`** to visualize the common ancestor.
2. Automate recurring conflict resolutions across feature branches using **`git rerere`** (Reuse Recorded Resolution).
3. Safely navigate and recover from **Detached HEAD** states without losing work.
4. Utilize **`git reflog`** as an infallible time-machine safety net to resurrect deleted branches and commits.
5. Isolate regressions and breaking bugs across thousands of commits in seconds using **`git bisect`**.
6. Safely reverse bad production commits using **`git revert`** (single commits and 3-way merge commits).

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Merge Conflict Anatomy & diff3](./01-Merge-Conflict-Anatomy-and-diff3-Visualization.md) | Understanding conflict markers, base ancestor diffs, GUI merge tools |
| 02 | [git rerere: Reuse Recorded Resolution](./02-git-rerere-Reuse-Recorded-Resolution.md) | Recording and auto-applying past conflict resolutions |
| 03 | [Detached HEAD State & Recovery](./03-Detached-HEAD-State-Anatomy-and-Safe-Recovery.md) | Why detached HEAD happens, committing in detached state, attaching to branch |
| 04 | [git reflog: The Ultimate Safety Net](./04-git-reflog-The-Ultimate-DevOps-Safety-Net.md) | Head reflogs, branch reflogs, recovering deleted commits, reflog expiry |
| 05 | [Automated Bug Hunting with git bisect](./05-Automated-Bug-Hunting-with-git-bisect.md) | Binary search debugging, automating tests with `git bisect run` |
| 06 | [Reverting Commits & Merges](./06-Reverting-Commits-and-3-Way-Merge-Reversals.md) | `git revert -m 1`, cleanly backing out broken releases in production |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Hotfix reverted incorrectly wiping feature branch, bisect finding race condition |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Aborting stuck rebases/merges, clearing broken rebase-apply state |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Simulating merge conflict, enabling rerere, and running an automated bisect script |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Disaster recovery commands, reflog rescue cheat sheet, bisect syntax |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Git Hooks](../06-Git-Hooks-and-Automation/README.md) | [README](./README.md) | [01 - Conflict Anatomy](./01-Merge-Conflict-Anatomy-and-diff3-Visualization.md) |
