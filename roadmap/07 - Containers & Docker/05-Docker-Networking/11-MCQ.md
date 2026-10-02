# 11 - Multiple-Choice Assessment (MCQ)

### 1. What loopback IP address is assigned to Docker's internal embedded DNS server?
- [ ] A) `127.0.0.1`
- [ ] B) `172.17.0.1`
- [x] C) `127.0.0.11`
- [ ] D) `10.0.0.2`

<details>
<summary><b>Explanation</b></summary>
Docker runs its embedded DNS resolver on <code>127.0.0.11:53</code> for all user-defined bridge and overlay networks.
</details>

---

### 2. In which iptables chain should custom administrator firewall rules be placed to prevent Docker from bypassing them?
- [ ] A) `INPUT`
- [ ] B) `DOCKER`
- [x] C) `DOCKER-USER`
- [ ] D) `FORWARD`

<details>
<summary><b>Explanation</b></summary>
Docker reserves the <code>DOCKER-USER</code> chain specifically for administrator rules; it is evaluated before any Docker-generated NAT rules.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
