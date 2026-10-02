# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Caddy Diagnostic CLI Commands

```bash
# Validate Caddyfile syntax without restarting
caddy validate --config /etc/caddy/Caddyfile

# Format Caddyfile to standard indentation
caddy fmt --overwrite /etc/caddy/Caddyfile

# Gracefully reload running Caddy process
caddy reload --config /etc/caddy/Caddyfile

# Inspect certificate storage on Linux
ls -la /var/lib/caddy/.local/share/caddy/certificates/
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Questions](./09-Interview-QA.md) |
