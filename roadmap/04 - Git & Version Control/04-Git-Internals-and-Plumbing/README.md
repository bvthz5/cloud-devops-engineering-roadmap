# Module 04: Git Internals and Plumbing Commands

Welcome to **Module 04: Git Internals and Plumbing Commands**. Beneath Git's everyday commands (`add`, `commit`, `checkout`) lies an elegant content-addressable filesystem and Directed Acyclic Graph (DAG) object store.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Deconstruct the inner layout of the **`.git` directory** (`objects/`, `refs/`, `HEAD`, `index`, `hooks/`).
2. Master the **4 Core Git Objects**: **Blobs**, **Trees**, **Commits**, and **Annotated Tags**.
3. Understand cryptographic object hashing, SHA-1 collision defense, and the transition to SHA-256.
4. Construct manual commits from scratch using low-level **Plumbing Commands** (`hash-object`, `cat-file`, `write-tree`, `commit-tree`, `update-ref`).
5. Optimize repository storage using **Packfiles**, delta compression, and garbage collection (`git gc`, `git prune`).
6. Navigate the Directed Acyclic Graph (DAG) and assess object reachability.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Inside the .git Directory](./01-Inside-the-dot-git-Directory-Anatomy.md) | Objects, refs, HEAD, index binary, logs, and config files |
| 02 | [The 4 Core Git Objects](./02-The-Four-Core-Git-Objects-Blobs-Trees-Commits-Tags.md) | Immutable content-addressable storage, header + size null byte hashing |
| 03 | [Plumbing vs. Porcelain Commands](./03-Plumbing-vs-Porcelain-Commands-Deep-Dive.md) | `hash-object`, `cat-file -p`, `write-tree`, `commit-tree`, `update-ref` |
| 04 | [Packfiles, Delta Compression & GC](./04-Packfiles-Delta-Compression-and-Garbage-Collection.md) | Loose objects vs packed objects (`.pack` and `.idx`), `git gc --prune=now` |
| 05 | [The Directed Acyclic Graph (DAG)](./05-The-Directed-Acyclic-Graph-DAG-and-Reachability.md) | Parent pointers, root commits, unreachable loose objects, dangling commits |
| 06 | [SHA-1 to SHA-256 Migration](./06-Cryptographic-Integrity-SHA1-to-SHA256-Migration.md) | SHAttered collision attack, hardened SHA-1 (sha1dc), Git SHA-256 object format |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Corrupted object database repair, CI runner packfile out-of-memory lockup |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | `git fsck --full`, recovering dangling commits from `.git/lost-found/` |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Building a commit by hand using only Git plumbing commands |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Object structure diagrams, plumbing command reference table |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Git Workflows](../03-Git-Workflows-Trunk-vs-GitFlow/README.md) | [README](./README.md) | [01 - Inside .git](./01-Inside-the-dot-git-Directory-Anatomy.md) |
