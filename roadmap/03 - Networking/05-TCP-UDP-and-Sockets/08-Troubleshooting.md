# 08 — Socket Troubleshooting Guide & Runbook

---

## 1. Essential `ss` Commands

```bash
# 1. Show all listening TCP and UDP sockets with PIDs
sudo ss -tulpn

# 2. Count connections by state
ss -ant | awk '{print $1}' | sort | uniq -c

# 3. Filter connections to specific port
ss -tn dport = :443

# 4. View overall socket memory statistics
ss -s
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
