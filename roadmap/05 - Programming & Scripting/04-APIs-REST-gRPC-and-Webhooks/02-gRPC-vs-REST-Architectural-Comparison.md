# 02 - gRPC vs. REST Architectural Comparison

## 1. Protocol Comparison Matrix

| Dimension | REST API | gRPC API |
|---|---|---|
| **Protocol** | HTTP/1.1 or HTTP/2 | Strictly **HTTP/2** (and HTTP/3) |
| **Payload** | JSON or XML (Human-readable text) | **Protocol Buffers (Binary)** |
| **Schema** | OpenAPI/Swagger (Optional) | `.proto` contract file (Mandatory) |
| **Performance** | Higher latency, larger payload size | **7-10x faster**, 60% smaller payloads |
| **Streaming** | Request/Response (SSE for 1-way) | **Bi-directional streaming** native |
| **Browser Support**| Universal native support | Requires gRPC-Web bridge / Envoy proxy |
| **Primary Use Case**| Public APIs, web frontends | **Internal microservices, SRE tooling** |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - REST API Architecture and Idempotency](./01-REST-API-Architecture-and-Idempotency.md) | [Index](../../../README.md) | [03 - Webhook Architecture and Reliable Delivery →](./03-Webhook-Architecture-and-Reliable-Delivery.md) |
