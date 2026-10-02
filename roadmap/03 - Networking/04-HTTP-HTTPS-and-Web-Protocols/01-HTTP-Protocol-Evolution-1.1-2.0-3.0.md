# 01 — HTTP Protocol Evolution: 1.1, 2.0, and 3.0

HTTP has evolved radically over three decades to meet the demands of modern high-concurrency web applications and mobile networks.

---

## 1. Architectural Comparison

```text
HTTP/1.1 (1997)               HTTP/2 (2015)                 HTTP/3 (2022)
Text-based protocol           Binary framing protocol       Binary framing over QUIC
TCP + TLS                     TCP + TLS 1.2+                UDP + QUIC (Built-in TLS 1.3)
Head-of-Line Blocking at App  Multiplexing over 1 TCP Conn  Zero Head-of-Line Blocking!
```

---

## 2. Deep Dive: Protocol Comparison

| Feature | HTTP/1.1 | HTTP/2 | HTTP/3 |
| :--- | :--- | :--- | :--- |
| **Transport Protocol**| TCP | TCP | **QUIC (UDP)** |
| **Data Format** | Plaintext ASCII | Binary Frames | Binary Frames |
| **Multiplexing** | No (Requires 6 parallel TCP conns)| Yes (Interleaves streams in 1 TCP conn)| Yes (Independent streams over QUIC) |
| **Head-of-Line (HoL)**| HoL blocking at HTTP level | Eliminated at HTTP; **Still blocks at TCP!**| **Completely eliminated!** |
| **Header Compression**| None (repeated raw text headers) | **HPACK** algorithm | **QPACK** algorithm |
| **Connection Handshake**| 2-3 RTT (TCP + TLS) | 2 RTT (TCP + TLS 1.2) | **0-RTT to 1-RTT** |
| **Connection Migration**| Broken on IP change (drops) | Broken on IP change (drops) | **Survives IP change** (Wi-Fi to 5G) |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - HTTP Request & Response](./02-HTTP-Request-and-Response-Anatomy.md) |
