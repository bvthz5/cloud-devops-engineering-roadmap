# 11 — Cryptography & PKI Interview Q&A

10 technical interview questions for DevOps, SRE, and Security roles.

---

### Q1: What is the difference between Symmetric and Asymmetric encryption?
**Answer:**
- **Symmetric Encryption (e.g. AES, ChaCha20):** Uses a single shared secret key for both encryption and decryption. It is extremely fast and suitable for bulk data transfer (disk encryption, video streaming, TLS data phase).
- **Asymmetric Encryption (e.g. RSA, Ed25519):** Uses a mathematically related key pair (Public Key to encrypt, Private Key to decrypt). It is computationally slow and primarily used for identity authentication, digital signatures, and key exchange during initial handshakes.

---

### Q2: What is Perfect Forward Secrecy (PFS)?
**Answer:**
PFS is a feature of secure communication protocols where a compromise of the server's long-term private key does not compromise past session keys. By generating ephemeral, temporary session keys for each connection using Ephemeral Diffie-Hellman (ECDHE), an eavesdropper who records encrypted traffic cannot decrypt historical data even if they obtain the server's private key in the future.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Troubleshooting](./10-Troubleshooting.md) | [README](./README.md) | [12 - Hands-On Practice](./12-Hands-On-Practice.md) |
