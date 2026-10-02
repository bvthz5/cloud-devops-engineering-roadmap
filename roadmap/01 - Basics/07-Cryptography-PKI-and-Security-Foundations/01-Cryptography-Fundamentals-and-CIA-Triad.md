# 01 — Cryptography Fundamentals and the CIA Triad

Information security is built on three core pillars: **Confidentiality, Integrity, and Availability (The CIA Triad)**.

---

## 1. The Core Security Properties

```text
               +-----------------------+
               |     CONFIDENTIALITY   |
               | (Encryption: AES/RSA) |
               +-----------+-----------+
                           |
             +-------------+-------------+
             |                           |
             v                           v
+-------------------------+ +-------------------------+
|        INTEGRITY        | |       AVAILABILITY      |
| (Hashes, HMAC, Sign)    | | (Redundancy, DDoS prot) |
+-------------------------+ +-------------------------+
```

1. **Confidentiality:** Only authorized parties can read the data (achieved via Encryption).
2. **Integrity:** Data cannot be modified or tampered with in transit without detection (achieved via Cryptographic Hashes, HMAC, Digital Signatures).
3. **Availability:** Systems and data remain accessible when needed (achieved via fault tolerance, load balancing, DDoS defense).
4. **Authentication:** Verifying the true identity of a party (achieved via Public Key Signatures, Certificates, Passwords).
5. **Non-repudiation:** A party cannot deny having sent a message (achieved via Digital Signatures).

---

## 2. Kerckhoffs's Principle

> *"A cryptographic system should be secure even if everything about the system, except the key, is public knowledge."*

In modern security, **never invent custom secret encryption algorithms** ("Security through obscurity" always fails). All industry-standard algorithms (AES, RSA, SHA-256) are open-source and mathematically scrutinized by thousands of cryptanalysts worldwide. Security relies strictly on the secrecy and randomness of the **cryptographic key**.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Symmetric Encryption](./02-Symmetric-Encryption-AES-and-ChaCha20.md) |
