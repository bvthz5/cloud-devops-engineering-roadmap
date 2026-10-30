# 09 - Interview Questions & Architectural Scenarios

### Q1: What makes TLS 1.3 significantly faster and more secure than TLS 1.2?
**Answer**: TLS 1.3 reduces the handshake from 2 round trips (2-RTT) to a single round trip (1-RTT) by combining cryptographic parameter negotiation with key exchange. It completely deprecates weak algorithms (RSA key exchange, SHA-1, CBC mode ciphers) and mandates Perfect Forward Secrecy (PFS) via ephemeral Diffie-Hellman.

### Q2: What is OCSP Stapling and what problem does it solve?
**Answer**: Under standard OCSP, client browsers query the Certificate Authority to check if a certificate is revoked, causing connection latency and privacy leaks. OCSP Stapling allows the web server to cache the signed revocation status from the CA and staple it directly to the TLS handshake, eliminating the client's extra network round trip.

### Q3: When is the ACME DNS-01 challenge required instead of HTTP-01?
**Answer**: DNS-01 is required when: 1) Requesting wildcard certificates (`*.example.com`), 2) Running servers behind private corporate firewalls where port 80 cannot be exposed to the public internet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
