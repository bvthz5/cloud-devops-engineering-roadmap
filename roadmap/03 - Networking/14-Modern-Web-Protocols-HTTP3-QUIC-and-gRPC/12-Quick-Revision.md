# 12 - Modern Protocols: Quick Revision Cheat Sheet

## Web Protocols Evolution

| Generation | Transport | Multiplexing | Security | Head-of-Line Blocking |
|---|---|---|---|---|
| **HTTP/1.1** | TCP | ❌ Pipeline only | TLS Optional | Full (At application layer) |
| **HTTP/2** | TCP | ✅ Streams | TLS De-facto | Partial (At TCP transport layer) |
| **HTTP/3** | **QUIC (UDP)** | ✅ Fully Independent | **TLS 1.3 Mandatory** | **Zero (Eliminated completely)** |

## Protocol Selection Cheat Sheet
- **Internal Microservices:** gRPC (Protobuf over HTTP/2)
- **High-Performance Public Web:** HTTP/3 (QUIC) with HTTP/2 fallback via `Alt-Svc`
- **Real-time AI Chat Streaming:** Server-Sent Events (SSE)
- **Two-Way Real-time Gaming / Collab:** WebSockets

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Self-Assessment MCQ](./11-MCQ.md) | [README](./README.md) | [Networking Index](../README.md) |
