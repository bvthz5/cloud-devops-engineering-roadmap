# Module 01: Git Architecture and Foundational Mechanics

Welcome to **Module 01: Git Architecture and Foundational Mechanics**. Git is the distributed version control foundation upon which all modern DevOps, CI/CD pipelines, and cloud collaboration operate.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Explain the architectural difference between **Centralized VCS** (SVN/Perforce) and **Distributed VCS** (Git).
2. Master the **Three Trees of Git**: The Working Directory, the Staging Area (Index), and the Git Repository (`HEAD`).
3. Configure global and repository-level identity, editor settings, and credential helpers.
4. Execute and understand the snapshot mechanics of `git init`, `git add`, `git commit`, `git status`, and `git diff`.
5. Safely unstage files, discard local working tree modifications, and restore file states using `git restore` and `git checkout`.
6. Inspect repository history with precision using advanced `git log` formatting, graphing, and filtering flags.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [VCS Evolution: Centralized vs. Distributed](./01-VCS-Evolution-Centralized-vs-Distributed.md) | SVN/Perforce vs Git; snapshot-based vs delta-based storage models |
| 02 | [The Three Trees of Git](./02-The-Three-Trees-Working-Directory-Index-and-HEAD.md) | Working Tree, Staging Area (Index), Repository; lifecycle transitions |
| 03 | [Git Configuration & Environments](./03-Git-Configuration-System-Global-and-Local.md) | `.gitconfig` hierarchy, SSH keys, credential caching, safe directories |
| 04 | [Basic Workflow: Staging & Committing](./04-Basic-Workflow-Staging-Committing-and-Status.md) | `git add -p` patch staging, atomic commits, conventional commit standards |
| 05 | [Diffing & State Inspection](./05-Diffing-and-State-Inspection-Working-vs-Staged.md) | `git diff`, `git diff --staged`, word-level diffs, ignore whitespace |
| 06 | [Log Mastery & History Visualization](./06-Log-Mastery-Filtering-Formatting-and-Graphing.md) | Custom pretty formats, author filtering, date ranges, graphing DAG trees |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Accidentally committing 5GB database file, unstaged file loss recovery |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Fixing wrong author email, unstaging files safely, recovering uncommitted work |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency DevOps/SRE Git architectural interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Setting up a multi-stage repository, patch staging, and historical log auditing |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Fast reference command table, staging lifecycle diagram, flag matrix |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Git Track Index](../README.md) | [README](./README.md) | [01 - VCS Evolution](./01-VCS-Evolution-Centralized-vs-Distributed.md) |
