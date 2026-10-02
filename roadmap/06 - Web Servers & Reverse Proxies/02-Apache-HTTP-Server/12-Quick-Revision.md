# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# Config test and module checks
apache2ctl configtest
apache2ctl -S
apache2ctl -M

# Debian/Ubuntu management
a2ensite mysite.conf && systemctl reload apache2
a2enmod rewrite ssl proxy proxy_http headers

# Security baseline
ServerTokens Prod
ServerSignature Off
TraceEnable Off
AllowOverride None
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Multiple-Choice Assessment](./11-MCQ.md) | [README](./README.md) | [03 - Reverse Proxy & Load Balancing](../03-Reverse-Proxy-and-Load-Balancing/README.md) |
