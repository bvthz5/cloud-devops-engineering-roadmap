# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Apache Diagnostic CLI Commands

```bash
# Test configuration syntax without restarting
apachectl configtest
# Output: Syntax OK

# List all parsed VirtualHosts, server names, and line numbers
apache2ctl -S

# Check which MPM is currently compiled and active
apache2ctl -V | grep -i mpm

# Inspect all loaded modules
apache2ctl -M
```

---

## 2. Resolving "Port 80 Already in Use" Conflict

```bash
# Identify process holding port 80 or 443
sudo ss -tulpn | grep -E ':(80|443)'

# If an orphaned process or competing Nginx instance is running:
sudo systemctl stop nginx
sudo systemctl restart apache2
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
