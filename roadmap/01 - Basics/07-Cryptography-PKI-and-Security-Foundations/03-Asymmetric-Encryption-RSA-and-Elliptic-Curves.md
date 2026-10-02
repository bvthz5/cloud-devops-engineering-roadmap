# 03 — Asymmetric Encryption: RSA and Elliptic Curves

Asymmetric cryptography uses a mathematically linked **Key Pair**:
- **Public Key:** Distributed openly to the world.
- **Private Key:** Kept strictly secret by the owner.

---

## 1. Two Primary Use Cases

1. **Encryption (Confidentiality):**
   - Sender encrypts data using Receiver's **Public Key**.
   - Only the Receiver can decrypt using their matching **Private Key**.
2. **Digital Signature (Authentication & Integrity):**
   - Signer signs data hash using their **Private Key**.
   - Anyone can verify the signature using the Signer's **Public Key**.

---

## 2. RSA vs Elliptic Curve (ECC)

| Metric | RSA (Rivest-Shamir-Adleman) | ECC / Ed25519 (Elliptic Curves) |
| :--- | :--- | :--- |
| **Mathematical Basis** | Factoring products of large prime numbers | Discrete logarithms on elliptic curves |
| **Recommended Key Size**| 3072 or 4096 bits | **256 bits** |
| **Key Generation Speed**| Slow (seconds) | Near instantaneous (milliseconds) |
| **Signature Size** | Large (~512 bytes) | Compact (~64 bytes) |
| **Hardware Efficiency** | Heavy CPU computation | Lightweight (ideal for microcontrollers & mobile)|

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Symmetric Encryption](./02-Symmetric-Encryption-AES-and-ChaCha20.md) | [README](./README.md) | [04 - Cryptographic Hashes & HMAC](./04-Cryptographic-Hash-Functions-and-HMAC.md) |
