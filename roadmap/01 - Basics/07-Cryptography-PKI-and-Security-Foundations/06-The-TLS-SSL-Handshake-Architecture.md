# 06 — The TLS 1.3 Handshake Architecture

Transport Layer Security (TLS) encrypts internet communications. TLS 1.3 (RFC 8446) overhauled the protocol to reduce connection latency and eliminate legacy insecure cryptographic ciphers.

---

## 1. TLS 1.3 1-RTT Handshake Sequence

```text
Client                                                  Server
  │                                                       │
  │ ─── 1. ClientHello ─────────────────────────────────> │
  │        - Supported TLS versions (TLS 1.3)             │
  │        - Supported Cipher Suites                      │
  │        - Server Name Indication (SNI: api.myapp.com)  │
  │        - Key Share (Client Diffie-Hellman public key) │
  │                                                       │
  │ <── 2. ServerHello ────────────────────────────────── │
  │        - Selected Cipher Suite (e.g. AES-256-GCM)     │
  │        - Server Key Share                             │
  │        - Server Certificate & Digital Signature       │
  │        - Finished                                     │
  │                                                       │
  │ [Both compute Shared Secret using ECDHE key share]    │
  │ [All subsequent traffic is encrypted with AES-GCM!]   │
  │                                                       │
  │ ─── 3. HTTP GET /api/v1/users (Encrypted!) ─────────> │
  │ <── 4. HTTP 200 OK (Encrypted!) ────────────────────  │
```

---

## 2. Perfect Forward Secrecy (PFS)

In legacy TLS with static RSA key exchange, if an attacker recorded encrypted network traffic for years and later compromised the server's private key, they could decrypt all past historical traffic!
**TLS 1.3 mandates Ephemeral Diffie-Hellman (ECDHE)**:
- A unique, temporary session key is generated per connection.
- The session key is deleted from RAM immediately after the session terminates.
- **Even if the server private key is leaked in the future, past recorded communications can NEVER be decrypted!**

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - PKI & X.509](./05-Public-Key-Infrastructure-PKI-and-X509.md) | [README](./README.md) | [07 - Password Security & KDFs](./07-Password-Security-Salts-and-KDFs.md) |
