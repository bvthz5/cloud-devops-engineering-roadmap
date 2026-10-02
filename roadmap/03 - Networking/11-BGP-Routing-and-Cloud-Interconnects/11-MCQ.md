# 11 - BGP Routing: Self-Assessment MCQs

### Q1. What underlying transport protocol and port does BGP use to exchange routing messages?
- A) UDP Port 520
- B) IP Protocol 89
- C) TCP Port 179
- D) SCTP Port 3868
<details><summary><b>View Answer</b></summary><b>Correct Answer: C</b><br>BGP relies on a reliable TCP connection on port 179.</details>

---

### Q2. Which BGP attribute is evaluated FIRST in the standard path selection algorithm?
- A) Local Preference
- B) Weight (Cisco proprietary)
- C) AS-Path Length
- D) MED
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>Weight is evaluated first (or Local Preference if non-Cisco standard BGP).</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
