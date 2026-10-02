# 11 — Multiple Choice Questions: OSI & TCP/IP

---

### Q1. What happens to an IPv4 packet when its TTL (Time to Live) reaches 0?
- [ ] A) It is returned to the sender via TCP retransmission
- [ ] B) The router discards the packet and sends an ICMP Time Exceeded message back to the source
- [ ] C) It is upgraded to IPv6
- [ ] D) The router resets its TTL to 64

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
When TTL reaches 0, routers discard the packet to prevent routing loops and inform the sender using ICMP Type 11.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
