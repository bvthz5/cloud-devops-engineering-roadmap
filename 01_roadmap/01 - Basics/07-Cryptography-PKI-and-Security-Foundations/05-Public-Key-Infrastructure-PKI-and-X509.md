# 05 — Public Key Infrastructure (PKI) and X.509 Certificates

Public Key Infrastructure is the framework of roles, policies, hardware, software, and procedures needed to create, manage, distribute, and revoke digital certificates.

---

## 1. How Digital Certificates Establish Trust

How do you know that `google.com`'s public key really belongs to Google and not an eavesdropping hacker?
A trusted third party called a **Certificate Authority (CA)** cryptographically signs the server's public key.

```text
[ Root CA ] (Self-Signed, pre-installed in OS trust store: /etc/ssl/certs/)
     │ Signs
     ▼
[ Intermediate CA ] (Issued by Root CA; used for daily signing)
     │ Signs
     ▼
[ End-Entity / Server Certificate ] (e.g. *.company.com)
     - Domain Name & SANs
     - Server Public Key
     - Expiration Date
     - Digital Signature of Intermediate CA
```

---

## 2. Anatomical Structure of an X.509 Certificate

- **Subject:** Organization name and common name (`CN=api.company.com`).
- **SAN (Subject Alternative Name):** Modern standard listing all valid domains (`DNS:api.company.com, DNS:*.company.com`).
- **Issuer:** The CA that signed this certificate (`Let's Encrypt Authority X3`).
- **Validity Period:** `Not Before` and `Not After` timestamps.
- **Public Key:** Algorithm (RSA 2048 or ECDSA P-256) and public key bytes.
- **Signature Algorithm:** e.g. `sha256WithRSAEncryption`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Cryptographic Hash Functions and HMAC](./04-Cryptographic-Hash-Functions-and-HMAC.md) | [Index](../../../README.md) | [06 - The TLS SSL Handshake Architecture →](./06-The-TLS-SSL-Handshake-Architecture.md) |
