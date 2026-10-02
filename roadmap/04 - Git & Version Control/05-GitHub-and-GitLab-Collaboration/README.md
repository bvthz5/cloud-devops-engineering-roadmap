# Module 05: GitHub and GitLab Enterprise Collaboration

Welcome to **Module 05: GitHub and GitLab Enterprise Collaboration**. Beyond local version control, modern software delivery relies on hosted Git platforms for code reviews, branch governance, release orchestration, and automated CI/CD gating.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Master remote tracking references: `origin`, `upstream`, and the mechanics of `git fetch` vs `git pull`.
2. Compare **Forking Workflows** (Open Source / Zero-Trust) vs **Shared Repository Workflows** (Enterprise Teams).
3. Design high-velocity **Pull Request (GitHub) & Merge Request (GitLab)** review processes.
4. Enforce enterprise **Branch Protection Rules**: Mandatory CI checks, linear history, signed commits, and merge queues.
5. Implement role-based governance and automatic reviewer assignment using **`CODEOWNERS`**.
6. Automate releases with **GitHub Releases**, changelog generation, and Semantic Versioning integration.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Remotes & Tracking Branches](./01-Remotes-and-Tracking-Branches-Architecture.md) | `refs/remotes/origin/*`, upstream remotes, tracking branch configuration |
| 02 | [Forking vs. Shared Branching](./02-Forking-Workflow-vs-Shared-Branch-Model.md) | Cross-repo collaboration, internal enterprise access controls |
| 03 | [PR and MR Review Workflows](./03-Pull-Requests-and-Merge-Requests-Review-Excellence.md) | Review threads, draft PRs, suggestion commits, approval thresholds |
| 04 | [Branch Protection & Merge Queues](./04-Branch-Protection-Rules-and-Merge-Queues.md) | Status checks, preventing race conditions with GitHub Merge Queues |
| 05 | [CODEOWNERS Architecture](./05-CODEOWNERS-Architecture-and-Governance.md) | Directory-level ownership, automated approval routing, compliance audits |
| 06 | [Releases & Semantic Automation](./06-GitHub-Releases-and-Release-Automation.md) | Automated release notes, semantic-release, asset attachments |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Broken build bypassing protection, CODEOWNERS deadlocking critical hotfix |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Syncing diverged forks, resolving out-of-date branch CI errors |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Configuring remotes, upstream sync, and writing a CODEOWNERS file |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Remotes cheat sheet, CODEOWNERS syntax summary, protection rules matrix |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Git Internals](../04-Git-Internals-and-Plumbing/README.md) | [README](./README.md) | [01 - Remotes Architecture](./01-Remotes-and-Tracking-Branches-Architecture.md) |
