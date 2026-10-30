# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which Envoy discovery API is responsible for streaming backend pod IP addresses and ports dynamically?
- [ ] A) LDS
- [ ] B) RDS
- [ ] C) CDS
- [x] D) EDS

<details>
<summary><b>Explanation</b></summary>
EDS (Endpoint Discovery Service) dynamically streams the individual IP addresses and ports of upstream cluster members.
</details>

---

### 2. What default port is used by Envoy for its administrative REST interface?
- [ ] A) 8080
- [ ] B) 9090
- [x] C) 15000
- [ ] D) 443

<details>
<summary><b>Explanation</b></summary>
Envoy's admin interface listens on port 15000 by convention, exposing endpoints like <code>/config_dump</code> and <code>/stats</code>.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
