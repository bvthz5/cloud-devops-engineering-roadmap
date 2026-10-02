# 09 - Git Architecture: Interview Questions & Answers

### Q1: What is the architectural difference between how Git stores file versions versus SVN?
**Answer:** SVN stores file revisions as a base version plus incremental line-by-line differences (deltas). Git stores files as complete immutable snapshots (Blobs) indexed by SHA-1/SHA-256 hashes. If a file does not change between commits, Git does not duplicate it; it simply points to the existing identical Blob object in the new Tree.

### Q2: What are the "Three Trees" in Git and what is their role?
**Answer:** 
1. **Working Directory:** The actual files on your filesystem that you edit.
2. **Index (Staging Area):** A binary file (`.git/index`) caching the exact state of files prepared for the next commit snapshot.
3. **Repository (`HEAD`):** The persistent database of commits and historical snapshots in `.git/objects`.

### Q3: What is the difference between `git diff` and `git diff --staged`?
**Answer:** `git diff` shows modifications in the Working Directory that have not yet been staged. `git diff --staged` (or `--cached`) shows changes that have been added to the Staging Area and are queued to be included in the next commit.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
