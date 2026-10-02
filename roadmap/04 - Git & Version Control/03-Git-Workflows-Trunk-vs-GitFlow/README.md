# Module 03: Git Branching Workflows: Trunk-Based vs. GitFlow

Welcome to **Module 03: Git Workflows: Trunk vs GitFlow**. The way an organization structures its branches dictates its deployment frequency, merge conflict overhead, and CI/CD automation success.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Deconstruct the traditional **GitFlow** branching model (feature, develop, release, hotfix, master) and evaluate its modern trade-offs.
2. Master **Trunk-Based Development (TBD)** and understand why it is the standard for high-performing DevOps and DORA elite teams.
3. Compare **GitHub Flow** and **GitLab Flow** (environment-based vs. release-based branches).
4. Decouple deployment from release using **Feature Flags** (Dark Launching).
5. Implement release management using **Semantic Versioning (SemVer 2.0)** and immutable Git tags.
6. Design an enterprise branching strategy tailored for continuous deployment to Kubernetes and Cloud environments.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [GitFlow Architecture & Lifecycle](./01-GitFlow-Architecture-and-Lifecycle.md) | Develop, release, hotfix branches; scheduled batch release models |
| 02 | [Trunk-Based Development (TBD)](./02-Trunk-Based-Development-and-Continuous-Integration.md) | Short-lived branches (<24h), direct merges to main, CI automated validation |
| 03 | [GitHub Flow & GitLab Flow Comparison](./03-GitHub-Flow-and-GitLab-Flow-Comparison.md) | Lightweight feature branch PRs, environment branches (staging/production) |
| 04 | [Feature Flags & Dark Launching](./04-Feature-Flags-Decoupling-Deploy-from-Release.md) | LaunchDarkly, Unleash, preventing long-lived branch merge hell with flags |
| 05 | [Semantic Versioning & Git Tags](./05-Semantic-Versioning-and-Git-Tags.md) | SemVer `MAJOR.MINOR.PATCH`, lightweight tags vs annotated cryptographic tags |
| 06 | [Enterprise Branching Strategy Design](./06-Designing-Enterprise-Branching-Strategies.md) | Matrix for choosing the right workflow for SaaS, Mobile, and Embedded systems |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | GitFlow merge paralysis delaying release, feature flag preventing Sev-1 outage |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Untangling backported hotfixes in GitFlow, handling stale long-lived branches |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Simulating Trunk-Based Development with feature toggles and tagged releases |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Workflow comparison matrix, SemVer rule summary, release flow cheat sheet |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Branching & Merging](../02-Branching-Merging-and-Rebasing/README.md) | [README](./README.md) | [01 - GitFlow](./01-GitFlow-Architecture-and-Lifecycle.md) |
