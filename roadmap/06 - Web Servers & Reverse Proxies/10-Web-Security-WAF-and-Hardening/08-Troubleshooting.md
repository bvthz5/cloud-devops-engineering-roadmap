# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Dissecting ModSecurity Audit Logs

When ModSecurity blocks a request, it logs full diagnostic traces to `/var/log/modsec_audit.log`:

```bash
# Search for blocked 403 transactions
grep -B 5 -A 20 "HTTP/1.1 403" /var/log/modsec_audit.log

# Extract the exact matched Rule ID and offending payload
grep -o "id "[0-9]*"" /var/log/modsec_audit.log
grep -o "data "[^"]*"" /var/log/modsec_audit.log
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Questions](./09-Interview-QA.md) |
