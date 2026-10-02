# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which Linux kernel namespace is responsible for isolating network interfaces, IP routing tables, and firewall rules?
- [ ] A) `CLONE_NEWNS`
- [x] B) `CLONE_NEWNET`
- [ ] C) `CLONE_NEWIPC`
- [ ] D) `CLONE_NEWPID`

<details>
<summary><b>Explanation</b></summary>
<code>CLONE_NEWNET</code> isolates network devices, IPv4/IPv6 stacks, IP routing tables, port bindings, and iptables rules.
</details>

---

### 2. Under cgroups v2, what file defines the hard memory limit for a container?
- [ ] A) `memory.limit_in_bytes`
- [x] B) `memory.max`
- [ ] C) `memory.hard_limit`
- [ ] D) `memory.high`

<details>
<summary><b>Explanation</b></summary>
In cgroups v2, <code>memory.max</code> sets the hard limit (exceeding it triggers OOMKilled). <code>memory.limit_in_bytes</code> was legacy cgroups v1.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
