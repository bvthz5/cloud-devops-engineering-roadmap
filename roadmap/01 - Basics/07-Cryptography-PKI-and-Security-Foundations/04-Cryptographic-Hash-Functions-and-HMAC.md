# 04 — Cryptographic Hash Functions and HMAC

A cryptographic hash function takes arbitrary-length input data and deterministically produces a fixed-size string of bytes (the digest). **Hashing is strictly one-way; you cannot reverse a hash back to plaintext!**

---

## 1. Three Essential Properties of a Cryptographic Hash

1. **Pre-image Resistance (One-Way):** Given a hash `H`, it is computationally infeasible to find input `m` such that `hash(m) == H`.
2. **Second Pre-image Resistance:** Given input `m1`, it is infeasible to find a different `m2` such that `hash(m1) == hash(m2)`.
3. **Collision Resistance:** It is infeasible to find *any* two arbitrary inputs `m1` and `m2` that produce the exact same hash.
4. **Avalanche Effect:** Changing a single bit in the input radically alters more than 50% of the output hash bits.

---

## 2. Hash Status & Recommendations

- **MD5 (128-bit):** **BROKEN.** Practical collisions can be generated in seconds on a laptop. Banned for security.
- **SHA-1 (160-bit):** **DEPRECATED.** Broken by Google/CWI in 2017 (SHAttered attack).
- **SHA-256 / SHA-512 (SHA-2 Family):** **The Current Standard.** Used in Bitcoin, Git (moving to SHA-256), TLS certificates, and Docker image digests (`sha256:7f...`).
- **SHA-3 (Keccak):** Alternative internal architecture (sponge function) resistant to length-extension attacks.

---

## 3. HMAC (Hash-based Message Authentication Code)

Standard hashing alone cannot prove message authenticity because anyone can recalculate a SHA-256 hash.
**HMAC** combines a cryptographic hash with a **shared secret key**:
```text
HMAC = Hash((Key ^ opad) || Hash((Key ^ ipad) || Message))
```
**Everyday DevOps Use:** Webhook signatures (GitHub, Stripe, Slack) use `X-Hub-Signature-256: sha256=...` to guarantee the webhook genuinely originated from GitHub and was not forged or replayed by an attacker.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Asymmetric Encryption](./03-Asymmetric-Encryption-RSA-and-Elliptic-Curves.md) | [README](./README.md) | [05 - PKI & X.509](./05-Public-Key-Infrastructure-PKI-and-X509.md) |
