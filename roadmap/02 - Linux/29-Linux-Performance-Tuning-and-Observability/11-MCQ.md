# 11 — Multiple Choice Questions: Performance Tuning

---

### Q1. What does it mean if `vmstat 1` consistently shows high non-zero numbers in the `si` and `so` columns?
- [ ] A) Sockets are being initialized
- [ ] B) The system is actively thrashing memory between RAM and Swap (Swap In / Swap Out) due to severe RAM exhaustion
- [ ] C) Software interrupts are handling network packets
- [ ] D) Storage inodes are full

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
<code>si</code> (swap-in) and <code>so</code> (swap-out) represent pages actively moved to/from swap disk. High values indicate severe memory starvation and thrashing.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
