# 02 — Modern SSH Keys: Ed25519 vs RSA

In modern cybersecurity, legacy algorithms like DSA and RSA-2048 are deprecated.

---

## 1. Why Ed25519 is the Gold Standard

- **Algorithm:** Twisted Edwards Curve 25519.
- **Key Length:** 256 bits (provides equivalent security to a 3072-bit RSA key).
- **Speed:** Generates and verifies signatures in milliseconds.
- **Resistant:** Immune to cache-timing attacks.

```bash
# Generate high-security Ed25519 key with 100 KDF rounds
ssh-keygen -t ed25519 -a 100 -C "admin@company.com"

# Permissions (Strictly enforced by SSH client!)
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - SSH Protocol Architecture and Handshake](./01-SSH-Protocol-Architecture-and-Handshake.md) | [Index](../../../README.md) | [03 - Hardening OpenSSH Server sshd_config →](./03-Hardening-OpenSSH-Server-sshd_config.md) |
