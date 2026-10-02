# 01 - TLS Handshake Architecture: 1.2 vs 1.3

## 1. TLS 1.2 vs TLS 1.3 Handshake Mechanics

```text
TLS 1.2 Handshake (2 Full Round Trips - 2-RTT):
Client                                               Server
  │                                                     │
  ├────── ClientHello (Supported Ciphers, ClientRandom)►│
  │                                                     ├─ ServerHello (Chosen Cipher, ServerRandom)
  │                                                     ├─ Certificate (Public Key Chain)
  │◄───── ServerKeyExchange (ECDHE Params) ─────────────┤
  ├────── ClientKeyExchange (PreMasterSecret) ─────────►│
  ├────── [ChangeCipherSpec] & Finished ───────────────►│
  │                                                     ├─ [ChangeCipherSpec] & Finished
  │◄───── Application Data Ready ───────────────────────┤
  (Total: 2 Round Trips before first HTTP byte is sent!)

TLS 1.3 Handshake (1 Single Round Trip - 1-RTT):
Client                                               Server
  │                                                     │
  ├────── ClientHello (Key Share Guess, ClientRandom) ─►│ (Client predicts server algorithm)
  │                                                     ├─ ServerHello (Server Key Share)
  │                                                     ├─ EncryptedCertificate
  │◄───── Finished ─────────────────────────────────────┤
  ├────── Application Data (HTTP GET) ─────────────────►│
  (Total: 1 Round Trip! Up to 50% lower handshake latency)
```

---

## 2. Perfect Forward Secrecy (PFS)

Historically, RSA key exchange allowed an attacker who recorded encrypted network traffic to decrypt **all past historical sessions** if the server's private key was compromised years later.

In modern TLS (and mandated by TLS 1.3), **Diffie-Hellman Ephemeral (ECDHE)** key exchange is used:
- A unique, temporary (ephemeral) session key is generated for every single TCP session.
- Even if the server's long-term private RSA/ECDSA key is stolen, past session traffic cannot be decrypted!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Certificate Authorities & PKI](./02-Certificate-Authorities-Chains-and-Trust-Stores.md) |
