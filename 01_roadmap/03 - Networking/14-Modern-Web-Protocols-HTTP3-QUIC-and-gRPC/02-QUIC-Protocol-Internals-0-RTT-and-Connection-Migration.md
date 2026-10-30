# 02 - QUIC Protocol Internals: 0-RTT and Connection Migration

## 1. 0-RTT Connection Resumption

In legacy HTTPS:
- TCP Handshake: 1 RTT
- TLS 1.3 Handshake: 1 RTT
- **Total: 2 RTTs** before the client can send an HTTP GET request.

In QUIC:
- Transport and cryptographic handshakes are merged into a single step.
- **Initial Connection:** **1 RTT** to complete both transport and encryption setup!
- **Reconnecting (0-RTT):** The client uses cached encryption secrets to send HTTP data in the **very first packet**!

```
QUIC 0-RTT Resumption:
Client ────────[ Initial Packet: Encrypted HTTP GET /api ]────────► Server
Client ◄───────[ Server Response: 200 OK + Data ]───────────────── Server
```

---

## 2. Connection Migration (Zero Mobile Drops)

In TCP, a connection is identified by the 4-tuple: `(Src IP, Src Port, Dest IP, Dest Port)`.
When a user walks out of their house and transitions from **Home Wi-Fi** to **Cellular 5G**:
- Their IP address changes.
- In TCP: All active connections instantly break and must re-handshake from scratch.
- In QUIC: The connection is identified by a unique **64-bit Connection ID (CID)** independent of IP or port. The smartphone continues streaming video seamlessly across network handovers!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Evolution from TCP to QUIC UDP Transport](./01-Evolution-from-TCP-to-QUIC-UDP-Transport.md) | [Index](../../../README.md) | [03 - HTTP3 Architecture and QPACK Compression →](./03-HTTP3-Architecture-and-QPACK-Compression.md) |
