# 05 - Real-Time Protocols: WebSockets vs. SSE vs. gRPC-Web

## 1. Real-Time Protocol Decision Matrix

| Protocol | Directionality | Transport | Best Suited For | Drawbacks |
|---|---|---|---|---|
| **WebSockets** | **Bidirectional** (Full Duplex) | Upgraded TCP connection | Collaborative apps (Google Docs, Figma), Multiplayer gaming, Chat | Bypasses HTTP semantics; complex load balancer connection management |
| **Server-Sent Events (SSE)** | **Unidirectional** (Server to Client) | Standard HTTP/1.1 or HTTP/2 | Stock tickers, real-time dashboards, **LLM AI Chat stream tokens (OpenAI API)** | Client cannot push data over the same channel |
| **gRPC-Web** | Unary & Server Streaming | HTTP/1.1 or HTTP/2 via Envoy proxy | Browser frontends calling internal gRPC microservices | Requires an Envoy proxy to bridge browser HTTP/1.1 to gRPC HTTP/2 trailers |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - gRPC Architecture Protocol Buffers and Streaming](./04-gRPC-Architecture-Protocol-Buffers-and-Streaming.md) | [Index](../../../README.md) | [06 - Observability and Debugging for Modern Protocols →](./06-Observability-and-Debugging-for-Modern-Protocols.md) |
