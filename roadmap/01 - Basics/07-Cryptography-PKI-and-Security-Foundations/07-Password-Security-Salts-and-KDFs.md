# 07 — Password Security, Salts, and Key Derivation (KDF)

Storing user or API passwords requires deliberate computational resistance against GPU-accelerated brute-force cracking.

---

## 1. Why Fast Hashes (MD5/SHA-256) Fail for Passwords

Modern GPUs can compute over **100 billion SHA-256 hashes per second**.
If an attacker steals a database table of SHA-256 password hashes:
1. They precompute **Rainbow Tables** (lookup tables of common passwords).
2. They run dictionary attacks that crack 8-character passwords in minutes.

---

## 2. Salt and Pepper

- **Salt:** A cryptographically random string (e.g. 16 bytes) generated uniquely for each user. Stored alongside the hash in the database.
  - Defeats Rainbow Tables because the attacker must recalculate hashes for every user's unique salt individually.
- **Pepper:** A secret key stored outside the database (e.g., in AWS KMS or HashiCorp Vault).

---

## 3. Slow Key Derivation Functions (KDFs)

Secure password hashes are **intentionally slow and memory-intensive**:
- **Argon2id:** **Winner of the Password Hashing Competition.** Memory-hard algorithm resistant to GPU/ASIC cracking. The modern recommendation.
- **bcrypt:** Standard algorithm incorporating variable work factors (`cost`).
- **scrypt:** Memory-hard algorithm preceding Argon2.
- **PBKDF2:** NIST-approved HMAC-based iterative function (requires 600,000+ iterations today).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - The TLS SSL Handshake Architecture](./06-The-TLS-SSL-Handshake-Architecture.md) | [Index](../../../README.md) | [08 - Entropy Randomness and dev urandom →](./08-Entropy-Randomness-and-dev-urandom.md) |
