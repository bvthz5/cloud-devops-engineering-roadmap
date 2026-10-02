# 01 — SSH Protocol Architecture and Cryptography

SSH-2 is a transport-layer protocol providing confidentiality, integrity, and authentication over untrusted networks.

---

## 1. The SSH Connection Lifecycle

```text
Client                                                  Server
  |                                                       |
  | ---------- 1. TCP 3-Way Handshake (Port 22) --------> |
  | <--------- 2. Protocol Version Exchange ------------> |
  |                                                       |
  | <--------- 3. Key Exchange (Diffie-Hellman) --------> |
  |    - Server sends Host Key (Verifies server identity) |
  |    - Both generate shared secret session key          |
  |    - Symmetric encryption (AES-GCM / ChaCha20) begins |
  |                                                       |
  | <========= 4. User Authentication Phase =============> |
  |    - Public Key (Ed25519 / RSA) or Certificate        |
  |    - Client signs challenge proving private key       |
  |                                                       |
  | <========= 5. Encrypted Interactive Session =========> |
```

---

## 2. Host Key Verification (`known_hosts`)

When connecting to a server for the first time, SSH presents the server's public key fingerprint. Stored in `~/.ssh/known_hosts`, it ensures that future connections are made to the legitimate server and protects against **Man-in-the-Middle (MitM)** attacks.

```bash
# Check fingerprint of remote server
ssh-keyscan -t ed25519 10.0.1.50 | ssh-keygen -lf -

# Remove an outdated/rotated host key
ssh-keygen -R 10.0.1.50
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (31-Linux-Backup-Archiving-and-rsync)](../31-Linux-Backup-Archiving-and-rsync/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Modern SSH Key Types Ed25519 vs RSA →](./02-Modern-SSH-Key-Types-Ed25519-vs-RSA.md) |
