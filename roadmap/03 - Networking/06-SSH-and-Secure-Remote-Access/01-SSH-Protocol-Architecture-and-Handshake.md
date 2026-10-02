# 01 — SSH Protocol Architecture and Handshake

SSH-2 is a three-tier architecture providing encrypted remote terminal sessions and file transfers over untrusted networks.

---

## 1. The Three SSH Protocol Layers

1. **Transport Layer Protocol (RFC 4253):** Handles server authentication, key exchange (Diffie-Hellman), encryption algorithm negotiation, and integrity hashing.
2. **User Authentication Protocol (RFC 4252):** Authenticates the client to the server (public key, certificate, or password).
3. **Connection Protocol (RFC 4254):** Multiplexes multiple logical channels (shell, SFTP, port forwarding) over the single encrypted connection.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Modern SSH Keys](./02-Modern-SSH-Keys-Ed25519-vs-RSA.md) |
