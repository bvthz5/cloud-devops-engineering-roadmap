# 04 - Git & Version Control Engineering

> Comprehensive, production-grade guide to Distributed Version Control, Branching Governance, Git Internals, GitOps, Enterprise Monorepos, Cryptographic Signing, and Large File Storage for Cloud, DevOps, and Platform Engineers.

---

## 🎯 Architecture & Roadmap Overview

Version control is the bedrock of modern software engineering and automated operations. In cloud-native and DevOps environments, Git serves not only as code storage, but as the single source of truth for declarative infrastructure (**GitOps**), compliance auditing, and continuous deployment triggers.

---

## 📌 Module Directory

| Module # | Domain | Core Focus Areas | Status |
|:---:|:---|:---|:---:|
| **01** | [01 - Git Architecture and Basics](./01-Git-Architecture-and-Basics/README.md) | Distributed vs Centralized, 3 Trees (Working, Index, HEAD), patch staging, log mastery | ✅ Complete |
| **02** | [02 - Branching, Merging, and Rebasing](./02-Branching-Merging-and-Rebasing/README.md) | Refs under the hood, Fast-Forward vs 3-way merge, rebase golden rule, interactive rebase, stashing | ✅ Complete |
| **03** | [03 - Git Workflows: Trunk vs GitFlow](./03-Git-Workflows-Trunk-vs-GitFlow/README.md) | Trunk-Based Development, GitFlow, GitHub/GitLab flow, Feature flags, SemVer 2.0 | ✅ Complete |
| **04** | [04 - Git Internals and Plumbing](./04-Git-Internals-and-Plumbing/README.md) | `.git` directory anatomy, 4 core objects (Blobs, Trees, Commits, Tags), Packfiles, GC, plumbing | ✅ Complete |
| **05** | [05 - GitHub & GitLab Collaboration](./05-GitHub-and-GitLab-Collaboration/README.md) | Remotes, tracking branches, Forking vs shared branches, Branch protection, Merge queues, CODEOWNERS | ✅ Complete |
| **06** | [06 - Git Hooks and Automation](./06-Git-Hooks-and-Automation/README.md) | Client vs Server hooks, `pre-commit` framework, automated secret scanning (`gitleaks`), push gates | ✅ Complete |
| **07** | [07 - Resolving Conflicts & Troubleshooting](./07-Resolving-Conflicts-and-Troubleshooting/README.md) | `diff3` conflict visualization, `git rerere`, Detached HEAD recovery, `git reflog`, `git bisect` | ✅ Complete |
| **08** | [08 - GitOps & Declarative Infrastructure](./08-GitOps-and-Declarative-Infrastructure/README.md) | OpenGitOps principles, Pull vs Push, Argo CD, Flux CD, Secrets (SOPS/Vault), Drift healing | ✅ Complete |
| **09** | [09 - Monorepos, Submodules & Large Git](./09-Monorepos-Submodules-and-Large-Scale-Git/README.md) | Monorepo trade-offs, `git submodule`, `git subtree`, Sparse Checkout, Blobless partial clones, Scalar | ✅ Complete |
| **10** | [10 - Git Security, Signing & Supply Chain](./10-Git-Security-Signing-and-Supply-Chain/README.md) | Commit spoofing threat, GPG and SSH key signing, `git filter-repo` secret purging, SLSA & SBOMs | ✅ Complete |
| **11** | [11 - Git LFS & Binary Artifacts](./11-Git-LFS-and-Artifact-Management/README.md) | Binary delta failure, Git LFS pointer architecture, file locking, repo migration, artifact registries | ✅ Complete |

---

## 🛠️ Module Structure Standards

Every module contains 13 dedicated, production-ready files:
1. `README.md` — Curriculum syllabus, prerequisites, and learning objectives.
2. `01` to `06` — Deep architectural engineering concepts with diagrams, internals, and CLI flags.
3. `07-Real-World-Scenarios.md` — Real production outages, SRE post-mortems, and incident analysis.
4. `08-Troubleshooting.md` — Diagnostic decision trees, recovery runbooks, and rescue commands.
5. `09-Interview-QA.md` — 10 high-frequency senior DevOps and SRE interview questions.
6. `10-Hands-On-Practice.md` — Real-world CLI labs with step-by-step terminal commands.
7. `11-MCQ.md` — 10 scenario-based self-assessment multiple-choice questions.
8. `12-Quick-Revision.md` — High-density cheat sheets and command reference tables.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Networking](../03%20-%20Networking/README.md) | [Master Roadmap Index](../00-Master-Index.md) | [01 - Git Basics](./01-Git-Architecture-and-Basics/README.md) |
