# 11 - CDN & Anycast: Self-Assessment MCQs

### Q1. What happens when multiple distinct datacenters announce the exact same IP address using BGP?
- A) IP address collision error; routers shut down the interfaces
- B) BGP Anycast routing directs each client to their topologically closest datacenter
- C) Packets are round-robined equally across all datacenters worldwide
- D) Only the datacenter with the lowest IP address accepts traffic
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>This is the fundamental principle of BGP Anycast.</details>

---

### Q2. Which Cache-Control directive allows serving an expired cached response while updating it asynchronously in the background?
- A) `must-revalidate`
- B) `stale-while-revalidate`
- C) `no-transform`
- D) `s-maxage`
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`stale-while-revalidate` serves stale cache immediately while triggering background refresh.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
