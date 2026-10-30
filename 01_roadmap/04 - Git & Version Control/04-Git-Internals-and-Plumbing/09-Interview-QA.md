# 09 - Git Internals: Interview Questions & Answers

### Q1: What does a Git Blob store, and does it include the filename or file permissions?
**Answer:** A Git Blob stores **only** the raw file contents (data). It does **not** store the filename or file permissions (mode). Filenames and permissions are stored inside **Tree objects** that point to the Blob's SHA hash.

### Q2: What is the difference between Porcelain and Plumbing commands in Git?
**Answer:** **Porcelain** commands (`add`, `commit`, `checkout`, `log`) are user-friendly interfaces designed for daily developer workflows. **Plumbing** commands (`hash-object`, `cat-file`, `write-tree`, `commit-tree`) are low-level UNIX-style commands that directly manipulate the underlying Git object database and refs.

### Q3: What is a Git Packfile and why is it necessary?
**Answer:** Storing every version of every file as an individual loose object consumes massive inode counts and disk space. A Packfile compresses hundreds of loose objects into a single binary file using delta compression (storing differences between similar file versions), drastically reducing disk and network transfer sizes.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
