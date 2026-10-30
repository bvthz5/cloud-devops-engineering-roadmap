# 09 — SSH Interview Q&A

10 technical interview questions for DevOps, SRE, and Security Engineering roles.

---

### Q1: Why is Ed25519 preferred over RSA for SSH keys?
**Answer:**
Ed25519 uses Twisted Edwards Curve 25519. It offers a 256-bit key length that provides equivalent security to a 3072-bit RSA key while being immune to timing side-channel attacks, generating keys in milliseconds, and producing compact 68-character public keys that fit easily into configuration management templates.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
