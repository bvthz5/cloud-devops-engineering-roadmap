# 08 — DNS Troubleshooting Guide & Runbook

---

## 1. The Diagnostic Toolkit

```bash
# 1. Hierarchical trace from root (.) servers to authoritative
dig +trace api.github.com

# 2. Query a specific upstream resolver directly (bypassing local cache)
dig @8.8.8.8 api.github.com

# 3. Reverse DNS lookup (IP to domain)
dig -x 140.82.121.4

# 4. Check DNSSEC signatures
dig +dnssec api.github.com

# 5. Flush local Linux DNS cache (systemd-resolved)
sudo resolvectl flush-caches
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
