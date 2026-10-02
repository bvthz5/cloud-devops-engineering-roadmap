# 12 — Quick Revision Cheat Sheet: SSH

---

## 1. Command Matrix

```bash
ssh-keygen -t ed25519 -a 100          # Generate key
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@server # Copy key
ssh -N -L 8080:dest:80 user@jumpbox   # Local forward
ssh -J user@jumpbox user@dest         # Jump through bastion
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (07-Firewalls-iptables-and-UFW) →](../07-Firewalls-iptables-and-UFW/01-Linux-Netfilter-Architecture-and-Hooks.md) |
