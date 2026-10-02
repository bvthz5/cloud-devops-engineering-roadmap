# 11 — Multiple Choice Questions: Linux Firewalls

---

### Q1. Which Netfilter hook is evaluated immediately after a packet arrives at an interface before any routing decision is made?
- [ ] A) INPUT
- [ ] B) FORWARD
- [ ] C) PREROUTING
- [ ] D) POSTROUTING

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: C</b><br>
The PREROUTING hook is triggered immediately upon packet ingress, making it the ideal place for Destination NAT (DNAT).
</details>

---

### Q2. Which custom iptables chain is officially reserved by Docker for administrator firewall rules?
- [ ] A) DOCKER-ADMIN
- [ ] B) DOCKER-USER
- [ ] C) DOCKER-FILTER
- [ ] D) DOCKER-OVERRIDE

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
Docker executes the <code>DOCKER-USER</code> chain before any internal container forwarding rules.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
