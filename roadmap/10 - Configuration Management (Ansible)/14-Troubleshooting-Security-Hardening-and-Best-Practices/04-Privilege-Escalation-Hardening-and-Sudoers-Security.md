# 04 - Privilege Escalation Hardening & Sudoers Security

Avoid `ALL=(ALL) NOPASSWD: ALL` in production. Scope sudo permissions explicitly to required binaries:
```text
ansible ALL=(root) NOPASSWD: /usr/bin/apt, /usr/bin/systemctl
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Security Hardening](./03-Security-Hardening-and-CIS-Benchmark-Automation.md) | [README](./README.md) | [05 - Idempotency Audit](./05-Ansible-Idempotency-Audit-and-Compliance-Reporting.md) |
