# Submodule 07: Cryptography, PKI, and Security Foundations

Security and cryptographic primitives form the bedrock of cloud infrastructure, DevSecOps, TLS encryption, SSH key management, API authentication, and zero-trust architectures.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Understand the **CIA Triad** (Confidentiality, Integrity, Availability) and non-repudiation.
- Differentiate **Symmetric (AES, ChaCha20)** and **Asymmetric (RSA, Ed25519)** encryption.
- Master cryptographic hash functions (**SHA-256, SHA-512**) and **HMAC** webhook signatures.
- Understand **Public Key Infrastructure (PKI)**, X.509 digital certificates, and certificate chains.
- Dissect the **TLS 1.3 Handshake** and **Perfect Forward Secrecy (PFS)**.
- Implement secure password storage with modern Key Derivation Functions (**Argon2id, bcrypt**).
- Explain Linux kernel entropy and difference between **`/dev/random` and `/dev/urandom`**.

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [Cryptography Fundamentals & CIA Triad](./01-Cryptography-Fundamentals-and-CIA-Triad.md) | Kerckhoffs's principle, threat models, and fundamental terminology | ✅ Complete |
| 02 | [Symmetric Encryption: AES & ChaCha20](./02-Symmetric-Encryption-AES-and-ChaCha20.md) | Block ciphers, modes (ECB vs CBC vs GCM), and AEAD authentication | ✅ Complete |
| 03 | [Asymmetric Encryption: RSA & ECC](./03-Asymmetric-Encryption-RSA-and-Elliptic-Curves.md) | Key pairs, RSA, Elliptic Curves (ECDSA, Ed25519), and performance | ✅ Complete |
| 04 | [Cryptographic Hashes & HMAC](./04-Cryptographic-Hash-Functions-and-HMAC.md) | Avalanche effect, collision resistance, SHA-256, and HMAC webhooks | ✅ Complete |
| 05 | [Public Key Infrastructure (PKI) & X.509](./05-Public-Key-Infrastructure-PKI-and-X509.md) | Certificate Authorities (CAs), SAN, chains of trust, and CRL/OCSP | ✅ Complete |
| 06 | [The TLS 1.3 Handshake Architecture](./06-The-TLS-SSL-Handshake-Architecture.md) | Handshake latency, Ephemeral Diffie-Hellman, PFS, and SNI | ✅ Complete |
| 07 | [Password Security & Key Derivation (KDF)](./07-Password-Security-Salts-and-KDFs.md) | Rainbow tables, salts, PBKDF2, bcrypt, scrypt, and Argon2id | ✅ Complete |
| 08 | [Entropy, Randomness & /dev/urandom](./08-Entropy-Randomness-and-dev-urandom.md) | PRNG vs CSPRNG, Linux kernel entropy pool, and hardware RNG | ✅ Complete |
| 09 | [Real-World Production Scenarios](./09-Real-World-Scenarios.md) | Expired root CA global outage, webhook replay attack mitigation | ✅ Complete |
| 10 | [Troubleshooting Guide & Runbook](./10-Troubleshooting.md) | Inspecting certificates with `openssl s_client`, verifying chains | ✅ Complete |
| 11 | [Interview Q&A](./11-Interview-QA.md) | 10 technical cryptography interview questions for DevOps & Security | ✅ Complete |
| 12 | [Hands-On Practice Labs](./12-Hands-On-Practice.md) | Create private CA, encrypt with AES-GCM, verify HMAC signatures | ✅ Complete |
| 13 | [Multiple Choice Questions (MCQ)](./13-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 14 | [Quick Revision Cheat Sheet](./14-Quick-Revision.md) | Cipher comparison table, OpenSSL command matrix, and key lengths | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Compilers & Runtimes](../06-Compilers-Linkers-and-Runtimes/README.md) | [01 - Basics Index](../README.md) | [01 - Cryptography Fundamentals](./01-Cryptography-Fundamentals-and-CIA-Triad.md) |
