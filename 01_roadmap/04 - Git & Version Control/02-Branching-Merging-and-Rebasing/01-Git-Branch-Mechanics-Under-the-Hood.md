# 01 - Git Branch Mechanics Under the Hood

## 1. What is a Git Branch?

In older VCS like SVN, creating a branch copied all project files into a new directory, taking minutes and consuming hundreds of megabytes.

In Git, **a branch is simply a lightweight, 41-byte text file** containing a 40-character commit hash!
- File location: `.git/refs/heads/<branch-name>`
- Creating a branch takes **1 millisecond** and uses almost zero disk space.

```
HEAD ────► refs/heads/feature ────► [ Commit C3 (9f2a4b) ]
                                            │
                                            ▼ Parent
refs/heads/main ──────────────────► [ Commit C2 (e4c1d0) ]
                                            │
                                            ▼ Parent
                                    [ Commit C1 (a1b2c3) ]
```

---

## 2. The `HEAD` Pointer

`HEAD` is a symbolic reference indicating which branch and commit is currently checked out:
- Read `HEAD`: `cat .git/HEAD` $ightarrow$ `ref: refs/heads/main`
- When you create a new commit, Git writes the new commit object, and updates the branch file pointed to by `HEAD` to contain the new hash.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (01-Git-Architecture-and-Basics)](../01-Git-Architecture-and-Basics/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Merge Strategies Fast Forward vs 3 Way →](./02-Merge-Strategies-Fast-Forward-vs-3-Way.md) |
