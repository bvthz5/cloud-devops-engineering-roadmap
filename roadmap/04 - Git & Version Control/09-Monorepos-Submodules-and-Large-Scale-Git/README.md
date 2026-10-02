# Module 09: Monorepos, Submodules, and Large-Scale Git Architecture

Welcome to **Module 09: Monorepos, Submodules, and Large-Scale Git Architecture**. When engineering organizations scale to thousands of microservices or multi-gigabyte codebases, traditional Git cloning and workflow patterns break down.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Evaluate the architectural trade-offs between **Monorepos** and **Polyrepos** for enterprise teams.
2. Master **`git submodule`** mechanics, `.gitmodules` management, and avoiding detached HEAD traps.
3. Compare `git submodule` vs. **`git subtree`** for third-party dependency vendor management.
4. Scale massive repositories using **Sparse Checkout (`git sparse-checkout`)** to download only relevant directories.
5. Accelerate CI/CD pipeline clones using **Partial Clones (`--filter=blob:none`)** and shallow clones (`--depth=1`).
6. Deploy Microsoft **Scalar** / VFS for Git for enterprise repositories with millions of files.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Monorepo vs. Polyrepo Architecture](./01-Monorepo-vs-Polyrepo-Architectural-Trade-Offs.md) | Code sharing, atomic cross-service refactoring, build caching tooling |
| 02 | [git submodule Mechanics & Pitfalls](./02-git-submodule-Mechanics-and-Pitfalls.md) | `.gitmodules`, commit pinning, `submodule update --init --recursive` |
| 03 | [git subtree vs. git submodule](./03-git-subtree-vs-git-submodule-Comparison.md) | In-tree dependency embedding, pushing upstream fixes from subtrees |
| 04 | [Sparse Checkout & Monorepo Scaling](./04-Sparse-Checkout-and-Monorepo-Scaling.md) | `git sparse-checkout set`, cone mode, working with subset of directories |
| 05 | [Partial Clones & Blobless Checkouts](./05-Partial-Clones-Blobless-and-Treeless-Clones.md) | `--filter=blob:none`, `--filter=tree:0`, saving 90% bandwidth in CI runners |
| 06 | [Git Scalar & VFS for Massive Scale](./06-Git-Scalar-and-Filesystem-Virtualization.md) | Microsoft Scalar, background maintenance, FSMonitor daemon integration |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Submodule pointer desync causing broken build, monorepo clone OOM in CI |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Fixing dirty submodule status, repairing broken `.gitmodules` URLs |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Configuring sparse-checkout and testing blobless clones on a multi-project repo |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Submodule command cheat sheet, sparse checkout syntax, clone optimization matrix |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - GitOps](../08-GitOps-and-Declarative-Infrastructure/README.md) | [README](./README.md) | [01 - Monorepo vs Polyrepo](./01-Monorepo-vs-Polyrepo-Architectural-Trade-Offs.md) |
