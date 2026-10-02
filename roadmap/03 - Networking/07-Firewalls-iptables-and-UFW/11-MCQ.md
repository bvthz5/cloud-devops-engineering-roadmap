# 11 - Firewalls & iptables: Self-Assessment MCQs

### Q1. Which Netfilter hook evaluates locally generated outbound packets before a routing decision?
- A) NF_INET_PRE_ROUTING
- B) NF_INET_LOCAL_OUT
- C) NF_INET_POST_ROUTING
- D) NF_INET_FORWARD
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>Outbound packets originating from local processes hit `NF_INET_LOCAL_OUT` first.</details>

---

### Q2. Which iptables table is responsible for Source NAT (MASQUERADE)?
- A) filter
- B) raw
- C) mangle
- D) nat
<details><summary><b>View Answer</b></summary><b>Correct Answer: D</b><br>The `nat` table handles both SNAT/MASQUERADE (in POSTROUTING) and DNAT (in PREROUTING).</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
