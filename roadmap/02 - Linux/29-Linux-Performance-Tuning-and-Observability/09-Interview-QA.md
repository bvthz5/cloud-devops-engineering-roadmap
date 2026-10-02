# 09 — Linux Performance Interview Q&A

10 technical interview questions for DevOps, SRE, and Performance Engineering roles.

---

### Q1: What does a high `%iowait` percentage in `top` mean?
**Answer:**
`%iowait` means the CPU was idle, but at least one thread on that CPU was blocked waiting for an outstanding disk or network I/O request to finish. It is often misunderstood as "CPU is doing I/O work", but the CPU is actually sitting idle. High `%iowait` indicates a **storage or disk subsystem bottleneck**, not a CPU capacity problem.

---

### Q2: What is the difference between `buffers` and `cached` in `free -m`?
**Answer:**
- `buffers`: Stores raw disk blocks (block device metadata, directory blocks) in memory.
- `cached`: Stores file contents read from disk (**PageCache**). When a file is read, its pages stay cached in RAM so future reads are instantaneous. Linux automatically reclaims this memory if applications need it.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
