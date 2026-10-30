# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Inspecting Live Certificates via `openssl`

```bash
# Connect and view the full certificate chain delivered by server
openssl s_client -connect example.com:443 -servername example.com -showcerts

# Inspect expiration date of remote server
echo | openssl s_client -connect example.com:443 -servername example.com 2>/dev/null | \
  openssl x509 -noout -dates

# Inspect SAN (Subject Alternative Names) of a local certificate file
openssl x509 -in /etc/nginx/ssl/cert.pem -noout -text | grep -A 2 "Subject Alternative Name"

# Verify that a private key matches a certificate (MD5 hashes MUST match!)
openssl x509 -noout -modulus -in cert.pem | md5sum
openssl rsa -noout -modulus -in key.pem | md5sum
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
