# 09 — Linux Storage Interview Q&A

10 technical interview questions for DevOps, SRE, and Systems Engineering roles.

---

### Q1: What is the difference between an Inode and a Data Block?
**Answer:**
An Inode is a data structure that stores metadata about a file (file size, ownership, access permissions, timestamps, and pointers to the physical disk blocks where the actual data resides). An Inode does not contain the filename or the file contents. Data Blocks are the fixed-size chunks of storage on the block device that store the actual byte contents of the file.

---

### Q2: Why can't an XFS filesystem be shrunk?
**Answer:**
XFS was designed from inception for massive scalability, high-parallelism I/O, and allocation groups. The internal b-tree index architecture of XFS does not support moving allocation groups or relocating data blocks backward to allow filesystem shrinkage. ext4 supports offline shrinking, but XFS only supports online growth (`xfs_growfs`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
