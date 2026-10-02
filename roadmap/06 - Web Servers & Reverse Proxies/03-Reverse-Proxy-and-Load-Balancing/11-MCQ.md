# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which upstream directive in Nginx designates a standby server that only receives traffic when all primary servers are down?
- [ ] A) `standby`
- [x] B) `backup`
- [ ] C) `failover`
- [ ] D) `secondary`

<details>
<summary><b>Explanation</b></summary>
The <code>backup</code> parameter marks a server as a standby node that is only passed traffic when all non-backup primary servers are unavailable.
</details>

---

### 2. Which protocol prepends a connection header to Layer 4 TCP streams to preserve the original client IP across proxies?
- [ ] A) BGP Protocol
- [x] B) PROXY Protocol
- [ ] C) X-Forwarded Protocol
- [ ] D) SNI Protocol

<details>
<summary><b>Explanation</b></summary>
The PROXY Protocol (developed by HAProxy) prepends a small header to the TCP stream containing client and server IP/port details without inspecting application layer data.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
