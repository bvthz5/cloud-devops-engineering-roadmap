# 01 — SSL/TLS and HTTPS Fundamentals

## 1. What is SSL/TLS and HTTPS?
**HTTPS (Hypertext Transfer Protocol Secure)** is the secure version of HTTP. It uses **TLS (Transport Layer Security)**—formerly known as **SSL (Secure Sockets Layer)**—to encrypt data exchanged between a web browser (client) and a web server over TCP port 443.

```
+-------------------------------------------------------------+
|               HTTP Application Layer (Port 80)              |
+-------------------------------------------------------------+
                               |
                   +-----------v-----------+
                   |  TLS Security Layer   |  <-- Encryption & Authentication
                   +-----------+-----------+
                               |
                               v
+-------------------------------------------------------------+
|                 TCP Transport Layer (Port 443)              |
+-------------------------------------------------------------+
```

## 2. Core Security Pillars of HTTPS

| Pillar | Description | How TLS Achieves It |
|---|---|---|
| **Confidentiality** | Prevents eavesdropping on data in transit. | Symmetric encryption (AES-256-GCM, ChaCha20). |
| **Integrity** | Prevents data tampering/modification in transit. | Message Authentication Codes (MAC / HMAC-SHA256). |
| **Authentication** | Verifies identity of the remote server. | Public Key Infrastructure (PKI) X.509 certificates. |

## 3. The TLS 1.3 Handshake Lifecycle

```
Client                                                   Server
  |                                                         |
  | -------- ClientHello (Key Share, Ciphers) -----------> |
  |                                                         |  1. Selects Cipher
  |                                                         |  2. Generates Key Share
  | <------- ServerHello, EncryptedExtensions, ----------- |  3. Sends Certificate
  |          Certificate, CertVerify, Finished              |
  |                                                         |
  | [Client Verifies Certificate against Trust Store]       |
  |                                                         |
  | -------- Finished ------------------------------------> |
  |                                                         |
  | <====== Encrypted Application Data (HTTP 200) ========> |
```

- **TLS 1.2**: Requires 2 Round Trips (2-RTT) to establish connection.
- **TLS 1.3**: Reduces handshake to **1 Round Trip (1-RTT)**, improving performance and removing legacy insecure cipher suites (RSA key exchange, 3DES, RC4).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README (Index)](./README.md) | [README](./README.md) | [02 - Let's Encrypt & ACME Architecture](./02-Lets-Encrypt-and-ACME-Protocol-Architecture.md) |
