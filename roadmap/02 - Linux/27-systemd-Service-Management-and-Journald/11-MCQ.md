# 11 — Multiple Choice Questions: systemd

---

### Q1. Why must `sudo systemctl daemon-reload` be executed after editing a unit file?
- [ ] A) To restart all running services
- [ ] B) To recompile systemd from source
- [ ] C) To instruct systemd PID 1 to re-read and parse unit configuration files from disk into memory
- [ ] D) To clear all journal logs

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: C</b><br>
systemd caches unit configurations in memory; changes on disk are not noticed until <code>daemon-reload</code> is called.
</details>

---

### Q2. Which systemd directive prevents a service from gaining root privileges via setuid binaries?
- [ ] A) `RootRestricted=yes`
- [ ] B) `NoNewPrivileges=true`
- [ ] C) `ProtectKernel=strict`
- [ ] D) `DropCaps=all`

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: B</b><br>
<code>NoNewPrivileges=true</code> ensures that the service process and all its children can never gain new privileges through setuid/setgid execution.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
