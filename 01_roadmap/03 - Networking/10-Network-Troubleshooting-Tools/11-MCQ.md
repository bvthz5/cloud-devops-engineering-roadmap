# 11 - Troubleshooting Tools: Self-Assessment MCQs

### Q1. Which command overrides DNS resolution for an HTTPS request without editing `/etc/hosts`?
- A) `curl --dns-server 1.1.1.1 https://example.com`
- B) `curl -H "Host: example.com" https://10.0.1.50`
- C) `curl --resolve example.com:443:10.0.1.50 https://example.com`
- D) `curl --proxy 10.0.1.50 https://example.com`
<details><summary><b>View Answer</b></summary><b>Correct Answer: C</b><br>`--resolve` properly handles both the IP routing and TLS SNI handshake correctly without header manipulation bugs.</details>

---

### Q2. Which tcpdump flag disables DNS name and port name resolution?
- A) `-v`
- B) `-nn`
- C) `-s0`
- D) `-q`
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`-n` disables host resolution, and `-nn` disables both host and port name resolution.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
