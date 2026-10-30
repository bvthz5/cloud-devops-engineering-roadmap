# 09 — Number Systems & Encoding Interview Q&A

10 technical interview questions for DevOps, SRE, and Infrastructure roles.

---

### Q1: Why is UTF-8 completely backward compatible with 7-bit ASCII?
**Answer:**
UTF-8 was deliberately engineered so that all ASCII characters (code points 0 through 127) have their most significant bit set to `0` (`0xxxxxxx`), matching their original 7-bit ASCII representation exactly. Any valid ASCII file is already a valid UTF-8 file byte-for-byte.

---

### Q2: What causes the error `/bin/bash^M: bad interpreter: No such file or directory`?
**Answer:**
The shell script was saved with Windows CRLF (`
`) line endings instead of Unix LF (`
`). The Linux kernel parses the shebang line up to the line feed character `
`, including the carriage return `` as part of the interpreter binary path (`/bin/bash`). Because no binary with a trailing `` exists, the kernel fails with `bad interpreter`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
