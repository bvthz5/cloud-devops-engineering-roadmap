# 01 - VCS Evolution: Centralized vs. Distributed

## 1. Centralized VCS (CVCS) vs. Distributed VCS (DVCS)

Prior to Git, version control systems like **Subversion (SVN)**, **CVS**, and **Perforce** relied on a central server architecture.

```
CENTRALIZED (SVN/CVS):
+--------------------+
|   Central Server   | ◄── Network required for: commit, log, diff, branch
| (Full History/DB)  |
+--------------------+
       ▲        ▲
       │        │
  [Client 1]  [Client 2] (Working copy only contains single snapshot)

DISTRIBUTED (Git):
+--------------------+               +--------------------+
|  Developer Laptop  | ◄── Full ───► |   GitHub / GitLab  |
|  (Complete Clone)  |   Sync (Push) |  (Complete Clone)  |
+--------------------+               +--------------------+
```

### Key Differences:
- **Offline Operations:** In DVCS, 95% of operations (`commit`, `log`, `diff`, `branch`, `merge`) execute entirely locally at disk speed with zero network dependency.
- **Resilience:** In DVCS, every single developer checkout is a full, cryptographic backup of the entire project repository and its history. If the central server is destroyed, any developer repository can restore the entire database.

---

## 2. Deltas vs. Stream of Snapshots

- **Legacy VCS (SVN):** Stores data as a base file plus a list of incremental file-based differences (deltas).
- **Git:** Thinks of data as a **stream of snapshots**. Every time you commit, Git takes a snapshot of what all files look like at that moment and stores a cryptographic reference to that snapshot. If a file has not changed, Git does not store it again—it simply creates a link to the previous identical file blob.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - The Three Trees](./02-The-Three-Trees-Working-Directory-Index-and-HEAD.md) |
