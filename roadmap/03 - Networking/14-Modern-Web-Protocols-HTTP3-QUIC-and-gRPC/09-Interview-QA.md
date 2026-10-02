# 09 - Modern Protocols: Interview Questions & Answers

### Q1: Why does HTTP/3 run over UDP instead of TCP?
**Answer:** TCP suffers from Head-of-Line (HoL) blocking at the transport layer, and TCP options are ossified by middleboxes across the global internet. Running QUIC over UDP allows rapid user-space transport innovation, 0-RTT handshakes, independent stream multiplexing, and seamless connection migration.

### Q2: What is the difference between Server-Sent Events (SSE) and WebSockets?
**Answer:** WebSockets is a full-duplex, bidirectional protocol running over a raw TCP connection. SSE is a lightweight unidirectional protocol (server-to-client only) that runs over standard HTTP with built-in reconnection capabilities, making it ideal for streaming events (like AI token generation).

### Q3: Why is gRPC faster than REST/JSON?
**Answer:**
1. **Compact Binary Encoding:** Protocol Buffers encode fields as tagged binary varints instead of verbose ASCII JSON text.
2. **Fast Serialization:** Compiles directly into native C++/Go/Rust machine code.
3. **HTTP/2 Multiplexing:** Multiple RPC calls share a single long-lived TCP connection without overhead.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
