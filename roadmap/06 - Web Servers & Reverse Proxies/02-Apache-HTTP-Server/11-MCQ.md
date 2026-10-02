# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which Apache MPM solves the problem of idle keepalive connections blocking worker threads?
- [ ] A) `mpm_prefork`
- [ ] B) `mpm_worker`
- [x] C) `mpm_event`
- [ ] D) `mpm_winnt`

<details>
<summary><b>Explanation</b></summary>
<code>mpm_event</code> delegates idle keepalive sockets to dedicated listener threads using OS event polling, returning worker threads to the pool to process active requests.
</details>

---

### 2. Which CLI command dumps the complete parsed VirtualHost tree with source file line numbers in Apache?
- [ ] A) `apachectl -V`
- [x] B) `apachectl -S`
- [ ] C) `apachectl -M`
- [ ] D) `apachectl -t`

<details>
<summary><b>Explanation</b></summary>
<code>apachectl -S</code> displays all virtual hosts, their listening IP/ports, server names, and the exact configuration file line where each is defined.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
