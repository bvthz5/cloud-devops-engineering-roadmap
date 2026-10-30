# 09 — Real-World Cryptography Production Scenarios

---

## Scenario 1: The Expired Intermediate CA Outage (Let's Encrypt DST Root CA X3)

### Incident Summary
In September 2021, millions of legacy devices, older OpenSSL versions (1.0.2), and microservices running Ubuntu 16.04 suddenly failed all outbound HTTPS API calls with:
`certificate has expired` or `SSL_do_handshake() failed`.

### Root Cause Analysis
Let's Encrypt's cross-signed root certificate (**DST Root CA X3**) expired. While modern browsers correctly followed the alternative unexpired chain (**ISRG Root X1**), older OpenSSL clients failed to validate alternative chains if any expired certificate existed in the local trust store (`/etc/ssl/certs/`).

### Production Solution
Remove the expired certificate from the local trust store and rehash:
```bash
sudo sed -i 's|mozilla/DST_Root_CA_X3.crt|!mozilla/DST_Root_CA_X3.crt|' /etc/ca-certificates.conf
sudo update-ca-certificates
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Entropy Randomness and dev urandom](./08-Entropy-Randomness-and-dev-urandom.md) | [Index](../../../README.md) | [10 - Troubleshooting →](./10-Troubleshooting.md) |
