# 12 — Hands-On Practice Labs: Cryptography & PKI

---

## Lab 1: Generating a Self-Signed CA and Server Certificate with OpenSSL

```bash
# 1. Generate Root CA Private Key & Certificate
openssl genrsa -out rootCA.key 4096
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.crt -subj "/CN=MyPrivateCA"

# 2. Generate Server Private Key & Certificate Signing Request (CSR)
openssl genrsa -out server.key 2048
openssl req -new -key server.key -out server.csr -subj "/CN=localhost"

# 3. Sign the Server Certificate with your private CA
openssl x509 -req -in server.csr -CA rootCA.crt -CAkey rootCA.key -CAcreateserial -out server.crt -days 365 -sha256

# 4. Verify the certificate against your CA!
openssl verify -CAfile rootCA.crt server.crt
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Interview Q&A](./11-Interview-QA.md) | [README](./README.md) | [13 - Multiple Choice Questions](./13-MCQ.md) |
