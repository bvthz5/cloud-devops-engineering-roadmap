# 02 — Symmetric Encryption: AES and ChaCha20

Symmetric encryption uses the **exact same secret key** for both encryption and decryption. It is extremely fast and computationally efficient.

---

## 1. AES (Advanced Encryption Standard)

AES is a NIST-standardized block cipher that processes data in fixed **128-bit (16-byte) blocks** using key sizes of 128, 192, or 256 bits.

### Cipher Modes Compared:
- **ECB (Electronic Codebook):** **DANGEROUS.** Encrypts identical plaintext blocks into identical ciphertext blocks, preserving visual data patterns (the famous "ECB Linux Penguin"). **Never use in production!**
- **CBC (Cipher Block Chaining):** XORs each plaintext block with the previous ciphertext block using an Initialization Vector (IV). Prone to padding oracle attacks if not paired with HMAC.
- **GCM (Galois/Counter Mode):** **The Modern Industry Standard.** A stream-like mode that provides **AEAD (Authenticated Encryption with Associated Data)**. It encrypts the data AND produces an authentication tag to guarantee integrity and detect tampering in a single operation!

---

## 2. ChaCha20-Poly1305

A modern stream cipher designed by Daniel J. Bernstein:
- Paired with Poly1305 for authentication (AEAD).
- **3x faster than AES on CPUs lacking hardware AES instructions** (e.g., mobile devices, embedded IoT, older cloud instances).
- Widely used in WireGuard VPN and TLS 1.3.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Cryptography Fundamentals and CIA Triad](./01-Cryptography-Fundamentals-and-CIA-Triad.md) | [Index](../../../README.md) | [03 - Asymmetric Encryption RSA and Elliptic Curves →](./03-Asymmetric-Encryption-RSA-and-Elliptic-Curves.md) |
