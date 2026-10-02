# 02 - The Three Trees: Working Directory, Index, and HEAD

## 1. The Core Architecture

Git manages your project files across three distinct states (often called the "Three Trees"):

```
+-------------------------------------------------------------------------------+
|                                  LOCAL MACHINE                                |
|                                                                               |
|  +--------------------+    git add     +--------------------+   git commit    +--------------------+  |
|  | Working Directory  | ─────────────► | Staging Area/Index | ──────────────► |  Repository (HEAD) |  |
|  | (Raw files on disk)| ◄───────────── | (.git/index file)  | ◄────────────── | (Committed history)|  |
|  +--------------------+  git restore   +--------------------+  git reset/rev  +--------------------+  |
+-------------------------------------------------------------------------------+
```

| Area | Location | Purpose |
|---|---|---|
| **Working Directory** | Your local filesystem checkout | Sandboxed local files where you write and test code. Contains untracked and modified files. |
| **Staging Area (Index)** | Single binary file at `.git/index` | The preparation zone. Houses the exact snapshot of changes queued to become the next commit. |
| **Repository (`HEAD`)** | The `.git/objects/` store | The permanent history. `HEAD` is a pointer to your current branch and latest commit. |

---

## 2. The Lifecycle of a File in Git

```
             Untracked
                 │
           [git add]
                 ▼
  Tracked ──► Staged ──► [git commit] ──► Unmodified
                 ▲                               │
                 │                               ▼
                 └──────── [git add] ◄────── Modified
```

1. **Untracked:** Git has no record of the file (newly created file).
2. **Tracked & Unmodified:** File matches the snapshot recorded in `HEAD`.
3. **Modified:** File has edits in working directory that are not yet staged.
4. **Staged:** Changes added to `.git/index`, prepared for the next snapshot.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - VCS Evolution](./01-VCS-Evolution-Centralized-vs-Distributed.md) | [README](./README.md) | [03 - Git Configuration](./03-Git-Configuration-System-Global-and-Local.md) |
