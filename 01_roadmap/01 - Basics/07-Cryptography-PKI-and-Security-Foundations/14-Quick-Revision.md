# 14 — Quick Revision Cheat Sheet: Cryptography & PKI

---

## 1. Quick Reference Matrix

| Need | Recommended Algorithm | Key Length / Output |
| :--- | :--- | :--- |
| **Symmetric Bulk Encryption** | AES-256-GCM or ChaCha20-Poly1305 | 256 bits |
| **Asymmetric Signatures / SSH** | Ed25519 (or RSA-4096) | 256 bits (or 4096 bits) |
| **Cryptographic Hash** | SHA-256 or SHA-512 | 256 / 512 bits |
| **Password Storage** | Argon2id (or bcrypt) | Dynamic memory & cost |
| **API Webhook Authentication** | HMAC-SHA256 | 256 bits |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 13 - MCQ](./13-MCQ.md) | [Index](../../../README.md) | [Next Module (08-Data-Structures-Algorithms-and-System-Design) →](../08-Data-Structures-Algorithms-and-System-Design/01-Algorithmic-Complexity-and-Big-O-Notation.md) |
