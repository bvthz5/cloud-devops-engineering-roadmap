# 10 - Hands-On Practice Labs

## Lab 1: End-to-End Self-Signed Root CA and mTLS Authentication

### Objective
Create a private Root CA, generate a signed client certificate, and configure Nginx to reject all requests that do not present a verified client certificate.

### Implementation
```bash
# 1. Create Private Root CA
openssl genrsa -out ca.key 4096
openssl req -x509 -new -nodes -key ca.key -sha256 -days 365 -out ca.crt \
  -subj "/CN=Enterprise Internal CA/O=DevOps"

# 2. Generate Client Key and Certificate Request
openssl genrsa -out client.key 2048
openssl req -new -key client.key -out client.csr -subj "/CN=DeveloperBob/O=Engineering"

# 3. Sign Client Certificate with Root CA
openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
  -out client.crt -days 90 -sha256

# 4. Test with curl
# Fails without client cert:
curl -k https://localhost:443
# Succeeds with mTLS:
curl -k --cert client.crt --key client.key https://localhost:443
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
