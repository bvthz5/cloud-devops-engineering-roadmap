# 11 — Multiple Choice Questions: TCP & UDP

---

### Q1. What does a non-zero `Recv-Q` on a listening socket in `ss -tulpn` indicate?
- [ ] A) Network congestion on the physical interface
- [ ] B) Connections have completed the 3-way handshake and are backed up in the kernel accept queue waiting for the application to call `accept()`
- [ ] C) Packets are being dropped by iptables
- [ ] D) Socket buffer is empty

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
For listening sockets, `Recv-Q` is the number of established connections queued waiting for the application to accept them.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
