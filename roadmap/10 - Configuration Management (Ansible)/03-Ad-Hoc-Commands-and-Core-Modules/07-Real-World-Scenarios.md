# 07 - Real-World Scenarios: Emergency Security Patching

Executing an ad-hoc emergency glibc security patch update across 500 servers simultaneously using `forks=50`:
```bash
ansible all -f 50 -m apt -a "name=libc6 state=latest" --become
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Utility Modules](./06-Network-and-Utility-Modules-ping-uri-get_url-stat.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
