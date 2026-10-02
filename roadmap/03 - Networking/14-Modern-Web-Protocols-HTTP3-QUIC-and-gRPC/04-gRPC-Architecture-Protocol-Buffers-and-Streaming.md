# 04 - gRPC Architecture, Protocol Buffers, and Streaming

## 1. Why gRPC Replaces REST/JSON in Microservices

| Dimension | REST over JSON | gRPC over Protocol Buffers |
|---|---|---|
| **Data Format** | Human-readable text (Verbose, large payload) | Binary format (Compact, extremely fast serialization) |
| **Contract Definition** | OpenAPI / Swagger (Optional, loosely enforced) | Strict `.proto` schema file (Strictly enforced) |
| **Code Generation** | Manual or third-party plugins | First-class native code generation across 10+ languages |
| **Transport** | Typically HTTP/1.1 or HTTP/2 | Strictly **HTTP/2** (and HTTP/3) multiplexed |
| **Performance** | Baseline | **Up to 7-10x faster serialization, 50% less CPU** |

---

## 2. Defining a Service with Protocol Buffers (`.proto`)

```protobuf
syntax = "proto3";

package payment;

service PaymentService {
    // Unary RPC: Single request -> Single response
    rpc ProcessPayment (PaymentRequest) returns (PaymentResponse);

    // Server Streaming: Single request -> Continuous stream of updates
    rpc StreamTransactions (TransactionFilter) returns (stream Transaction);
}

message PaymentRequest {
    string account_id = 1;
    double amount = 2;
    string currency = 3;
}

message PaymentResponse {
    string transaction_id = 1;
    bool success = 2;
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - HTTP/3 & QPACK](./03-HTTP3-Architecture-and-QPACK-Compression.md) | [README](./README.md) | [05 - Real-Time Protocols](./05-Real-Time-Protocols-WebSockets-vs-SSE-vs-gRPC-Web.md) |
