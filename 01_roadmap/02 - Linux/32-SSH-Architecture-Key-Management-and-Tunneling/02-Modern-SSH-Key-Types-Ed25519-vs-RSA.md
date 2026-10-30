# 02 — Modern SSH Key Types: Ed25519 vs RSA

In modern cybersecurity, legacy algorithms like DSA and RSA-1024 are deprecated due to vulnerability to factorization attacks.

---

## 1. Key Algorithm Comparison

| Algorithm | Key Length | Security Level | Speed | Modern Recommendation |
| :--- | :---: | :---: | :---: | :--- |
| **DSA** | 1024-bit | Broken | Slow | **Banned** (Disabled in modern OpenSSH) |
| **RSA** | 2048-bit | Weak | Moderate | Deprecated |
| **RSA** | 4096-bit | Strong | Slower | Legacy fallback compatibility only |
| **ECDSA** | 256/384-bit | Strong | Fast | Good, but NIST curves have potential flaws |
| **Ed25519** | 256-bit | **Maximum** | **Fastest** | **Industry Gold Standard** (Edwards-curve 25519) |

---

## 2. Generating Secure Ed25519 Keys

```bash
# Generate high-security Ed25519 key with 100 key-derivation rounds (resists brute-force)
ssh-keygen -t ed25519 -a 100 -C "devops-admin@company.com"

# Copy public key to remote server
ssh-copy-id -i ~/.ssh/id_ed25519.pub user@10.0.1.50

# Ensure strict file permissions (SSH will refuse keys with open permissions!)
chmod 700 ~/.ssh
chmod 600 ~/.ssh/id_ed25519
chmod 644 ~/.ssh/id_ed25519.pub
chmod 600 ~/.ssh/authorized_keys
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - SSH Protocol Architecture and Cryptography](./01-SSH-Protocol-Architecture-and-Cryptography.md) | [Index](../../../README.md) | [03 - OpenSSH Server Hardening sshd_config →](./03-OpenSSH-Server-Hardening-sshd_config.md) |
