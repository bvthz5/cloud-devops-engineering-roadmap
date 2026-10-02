# 09 — TCP & UDP Interview Q&A

10 technical interview questions for DevOps, SRE, and Infrastructure roles.

---

### Q1: What is the cause of a large number of sockets stuck in `CLOSE_WAIT` state?
**Answer:**
`CLOSE_WAIT` indicates that the remote peer sent a `FIN` packet to close the connection, and the local kernel acknowledged it. The socket is now waiting for the **local application program** to execute `close()` on the file descriptor. A buildup of `CLOSE_WAIT` sockets indicates an **application resource leak**, not a network or kernel configuration issue.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
