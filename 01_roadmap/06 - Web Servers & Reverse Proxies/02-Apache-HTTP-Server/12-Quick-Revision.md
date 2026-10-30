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
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (03-Reverse-Proxy-and-Load-Balancing) →](../03-Reverse-Proxy-and-Load-Balancing/01-Forward-Proxy-vs-Reverse-Proxy-Architecture.md) |
