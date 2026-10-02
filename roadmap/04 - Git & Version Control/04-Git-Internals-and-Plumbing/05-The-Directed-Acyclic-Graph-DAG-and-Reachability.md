# 05 - The Directed Acyclic Graph (DAG) and Reachability

## 1. The DAG Structure

Git's commit history is a **Directed Acyclic Graph (DAG)**:
- **Directed:** Parent pointers point backward from child to parent.
- **Acyclic:** Impossible to form a circular loop (because a parent commit hash is cryptographically immutable and calculated before the child commit exists).

```
   [ Commit C1 ] ◄──── [ Commit C2 ] ◄──── [ Commit C3 (main) ]
         ▲
         │
   [ Commit C4 ] ◄──── [ Commit C5 (feature) ]
```

---

## 2. Object Reachability & Dangling Commits

A commit is **Reachable** if a branch pointer (`refs/heads/*`), tag (`refs/tags/*`), or `HEAD` can trace back to it.
When you delete a branch (`git branch -D feature`), Git does **NOT delete the commits immediately**. The commits become **Dangling / Unreachable**:
- Unreachable commits remain safe on disk in `.git/objects/` for at least **30 to 90 days** (controlled by `gc.reflogExpire` and `gc.pruneExpire`).
- You can recover any dangling commit using `git reflog` or `git fsck`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Packfiles Delta Compression and Garbage Collection](./04-Packfiles-Delta-Compression-and-Garbage-Collection.md) | [Index](../../../README.md) | [06 - Cryptographic Integrity SHA1 to SHA256 Migration →](./06-Cryptographic-Integrity-SHA1-to-SHA256-Migration.md) |
