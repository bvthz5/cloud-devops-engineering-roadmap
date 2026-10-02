# 01 - Inside the .git Directory Anatomy

## 1. The Anatomy of `.git`

When you run `git init`, Git creates a hidden directory named `.git`. This directory contains the entire database, history, and configuration of the repository:

```text
.git/
├── HEAD                  # Points to the currently checked-out branch (e.g. ref: refs/heads/main)
├── config                # Repository-specific configuration settings (--local)
├── description           # Used by GitWeb (mostly unused in modern workflows)
├── hooks/                # Client-side shell scripts executed on actions (pre-commit, etc.)
├── info/
│   └── exclude           # Local file exclusion rules (like .gitignore, but never committed)
├── objects/              # The content-addressable object database
│   ├── [0-9a-f]{2}/      # Two-character hex directories housing 38-character loose objects
│   ├── info/
│   └── pack/             # Compressed packfiles (.pack) and index files (.idx)
└── refs/                 # Pointers to commit objects
    ├── heads/            # Local branch pointers (e.g. refs/heads/main)
    ├── tags/             # Release tags (e.g. refs/tags/v1.0.0)
    └── remotes/          # Remote tracking branches (e.g. refs/remotes/origin/main)
```

---

## 2. Inspecting the `index` and `HEAD`

- `.git/index`: A binary file caching filenames, file modes (permissions), timestamps, and SHA-1 object hashes of all files in the current Staging Area.
- `.git/logs/`: Stores the **reflog** (reference logs), recording every change to `HEAD` and branch pointers on your local machine.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (03-Git-Workflows-Trunk-vs-GitFlow)](../03-Git-Workflows-Trunk-vs-GitFlow/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - The Four Core Git Objects Blobs Trees Commits Tags →](./02-The-Four-Core-Git-Objects-Blobs-Trees-Commits-Tags.md) |
