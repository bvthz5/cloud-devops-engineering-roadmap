# 11 — Multiple Choice Questions: SSH Architecture

---

### Q1. Which permissions are required on the `~/.ssh/id_ed25519` private key file for SSH to accept it?
- [ ] A) `0777`
- [ ] B) `0644`
- [ ] C) `0600` (or `0400`)
- [ ] D) `0755`

<details>
<summary><b>View Answer</b></summary>
<b>Correct Answer: C</b><br>
OpenSSH strictly refuses to use private keys readable by group or others (must be <code>0600</code>).
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
