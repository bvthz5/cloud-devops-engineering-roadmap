# 05 — WebSockets, SSE, and gRPC Protocols

Traditional request-response HTTP is inefficient for real-time dashboards, chat apps, and microservice communications.

---

## 1. WebSockets vs Server-Sent Events (SSE)

| Feature | WebSockets | Server-Sent Events (SSE) |
| :--- | :--- | :--- |
| **Direction** | **Full-Duplex** (Bi-directional) | **Unidirectional** (Server to Client only) |
| **Transport** | Upgrades from HTTP (`101 Switching Protocols`) | Standard HTTP stream (`text/event-stream`) |
| **Protocol** | `ws://` or `wss://` (Custom framing) | Standard HTTP/1.1 or HTTP/2 |
| **Reconnection** | Must be implemented in client code | **Built-in browser auto-reconnect** |
| **Best Use Case** | Chat applications, multiplayer games | Live stock tickers, AI streaming responses (ChatGPT) |

---

## 2. gRPC (Google Remote Procedure Call)

Built on top of **HTTP/2**:
- Uses **Protocol Buffers (protobuf)** binary serialization (5x faster and smaller than JSON).
- Supports bi-directional streaming natively.
- Standard protocol for high-performance internal microservices and Kubernetes cluster components (etcd, kube-apiserver).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - HTTP Caching and Conditional Requests](./04-HTTP-Caching-and-Conditional-Requests.md) | [Index](../../../README.md) | [06 - CORS and Web Security Headers →](./06-CORS-and-Web-Security-Headers.md) |
