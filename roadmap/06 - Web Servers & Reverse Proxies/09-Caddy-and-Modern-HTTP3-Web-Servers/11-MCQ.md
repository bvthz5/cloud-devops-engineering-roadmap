# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which underlying transport protocol powers HTTP/3 in Caddy?
- [ ] A) TCP
- [x] B) UDP (via QUIC)
- [ ] C) SCTP
- [ ] D) WebSockets

<details>
<summary><b>Explanation</b></summary>
HTTP/3 runs over UDP utilizing the QUIC protocol, eliminating TCP head-of-line blocking and enabling 0-RTT connection resumption.
</details>

---

### 2. On which port does Caddy expose its administrative REST API by default?
- [ ] A) 8080
- [ ] B) 15000
- [x] C) 2019
- [ ] D) 9090

<details>
<summary><b>Explanation</b></summary>
Caddy's administrative REST API listens locally on port 2019 by default.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
