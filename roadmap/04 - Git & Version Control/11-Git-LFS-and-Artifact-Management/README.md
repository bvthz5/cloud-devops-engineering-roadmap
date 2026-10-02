# Module 11: Git LFS and Binary Artifact Management

Welcome to **Module 11: Git LFS and Binary Artifact Management**. Git was designed for text source code. Tracking large binary assets (AI/ML model weights, game textures, compiled ISOs, datasets) bloats repositories into gigabytes. Git Large File Storage (LFS) solves this bottleneck.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Explain why standard Git performs poorly on large binary files (inability to delta-compress binaries).
2. Deconstruct **Git LFS Architecture**: Small pointer files in Git vs binary blobs stored in remote object storage (S3/Artifactory).
3. Track and manage binary file extensions using `.gitattributes`.
4. Prevent unmergeable binary merge conflicts using **Git LFS File Locking** (`git lfs lock`).
5. Migrate historically bloated repositories to Git LFS using `git lfs migrate`.
6. Contrast Git LFS with enterprise artifact repositories (**JFrog Artifactory**, **AWS S3 / ECR**, **Nexus**).

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [The Large Binary File Problem](./01-The-Large-Binary-File-Problem-in-Git.md) | Delta compression failure, clone size explosion, Git performance degradation |
| 02 | [Git LFS Architecture & Pointers](./02-Git-LFS-Architecture-and-Pointer-Files.md) | Smudge & clean filters, pointer file anatomy, LFS server API |
| 03 | [Configuring LFS & .gitattributes](./03-Configuring-Git-LFS-and-gitattributes.md) | Tracking patterns (`*.psd`, `*.onnx`), `.gitattributes` syntax |
| 04 | [File Locking & Binary Conflict Prevention](./04-File-Locking-and-Binary-Conflict-Prevention.md) | `git lfs lock`, preventing corrupt binary merge conflicts in teams |
| 05 | [Repository Migration to Git LFS](./05-Migrating-Bloated-Repositories-to-Git-LFS.md) | `git lfs migrate import`, converting past commit history cleanly |
| 06 | [LFS vs. Dedicated Artifact Stores](./06-Git-LFS-vs-Dedicated-Artifact-Repositories.md) | When to use LFS vs Artifactory / Nexus / OCI registries / S3 |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | ML training model bloating repo to 100GB, LFS bandwidth quota outage |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Fixing `smudge filter failed`, missing LFS object errors on clone |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Tracking large files with Git LFS and inspecting the generated pointer file |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Git LFS CLI command cheat sheet, pointer structure, lock management |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Git Security](../10-Git-Security-Signing-and-Supply-Chain/README.md) | [README](./README.md) | [01 - Binary Problem](./01-The-Large-Binary-File-Problem-in-Git.md) |
