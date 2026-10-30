# 10 — Cryptography & PKI Troubleshooting Guide

---

## 1. Inspecting Live TLS Certificates with `openssl`

```bash
# 1. Connect to an HTTPS server and display the entire certificate chain
openssl s_client -connect api.github.com:443 -servername api.github.com -showcerts

# 2. Check certificate expiration dates without downloading
echo | openssl s_client -connect example.com:443 2>/dev/null | openssl x509 -noout -dates

# 3. Inspect a local certificate file (.crt or .pem)
openssl x509 -in server.crt -text -noout

# 4. Verify whether a private key matches a certificate (Both MD5 hashes MUST match!)
openssl x509 -noout -modulus -in server.crt | openssl md5
openssl rsa -noout -modulus -in server.key | openssl md5
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Real World Scenarios](./09-Real-World-Scenarios.md) | [Index](../../../README.md) | [11 - Interview QA →](./11-Interview-QA.md) |
