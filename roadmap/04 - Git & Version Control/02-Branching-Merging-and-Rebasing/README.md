# Module 02: Branching, Merging, Rebasing, and Cherry-Picking

Welcome to **Module 02: Branching, Merging, and Rebasing**. Branches in Git are ultra-lightweight 41-byte pointer files. Understanding how branches diverge, merge, and rebase is essential for clean collaboration in fast-moving engineering organizations.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Explain what a Git branch is internally (a 40-character SHA pointer in `.git/refs/heads/`).
2. Compare **Fast-Forward Merges** vs. **3-Way Merges** (`--no-ff`).
3. Master **`git rebase`** vs. **`git merge`**: Architectural trade-offs, linear history, and the Golden Rule of Rebasing.
4. Execute **Interactive Rebasing (`git rebase -i`)** to squash, reorder, edit, and drop commits cleanly.
5. Selectively apply single commits across branches using **`git cherry-pick`**.
6. Stash and manage work-in-progress code using `git stash` flags (`push`, `pop`, `apply`, `branch`).

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Git Branch Mechanics Under the Hood](./01-Git-Branch-Mechanics-Under-the-Hood.md) | Pointers in `.git/refs/heads`, HEAD tracking, branch creation speed |
| 02 | [Merge Strategies: Fast-Forward vs. 3-Way](./02-Merge-Strategies-Fast-Forward-vs-3-Way.md) | Fast-forward commits, merge base calculation, `--no-ff` merge commits |
| 03 | [Git Rebase Mechanics & The Golden Rule](./03-Git-Rebase-Mechanics-and-The-Golden-Rule.md) | Replaying commits, linear history, never rebasing public shared branches |
| 04 | [Interactive Rebasing Mastery](./04-Interactive-Rebasing-Squash-Fixup-and-Edit.md) | `pick`, `squash`, `fixup`, `reword`, `drop` operations, commit cleanup |
| 05 | [Cherry-Picking & Selective Porting](./05-Cherry-Picking-and-Selective-Commit-Porting.md) | `git cherry-pick`, cherry-pick ranges, conflict resolution during pick |
| 06 | [Git Stash Deep Dive](./06-Git-Stash-Deep-Dive-and-Work-in-Progress.md) | Stashing untracked files (`-u`), named stashes, inspecting stash diffs |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Hotfix cherry-picked to release, force-push overwriting teammate's branch |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Aborting failed rebases (`git rebase --abort`), recovering dropped stashes |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Interactive rebase cleanup and conflict resolution on feature branch |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Merge vs Rebase cheat sheet, interactive rebase commands reference |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Git Basics](../01-Git-Architecture-and-Basics/README.md) | [README](./README.md) | [01 - Branch Mechanics](./01-Git-Branch-Mechanics-Under-the-Hood.md) |
