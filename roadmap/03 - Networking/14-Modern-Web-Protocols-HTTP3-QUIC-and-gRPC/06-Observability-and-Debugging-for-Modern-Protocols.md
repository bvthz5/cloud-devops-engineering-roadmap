# 06 - Observability and Debugging for Modern Protocols

## 1. Testing gRPC with `grpcurl` and `grpcui`

```bash
# List all services exposed on a remote gRPC endpoint (with reflection enabled)
grpcurl -plaintext 127.0.0.1:50051 list

# Call a gRPC method with a JSON payload
grpcurl -plaintext -d '{"account_id": "acc_123", "amount": 99.50}' 127.0.0.1:50051 payment.PaymentService/ProcessPayment
```

---

## 2. Testing HTTP/3 with `curl`

Modern versions of `curl` support HTTP/3 via `ngtcp2` or `quiche`:
```bash
# Request specifically over HTTP/3 (QUIC UDP port 443)
curl --http3 -Iv https://cloudflare-quic.com
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Real-Time Protocols](./05-Real-Time-Protocols-WebSockets-vs-SSE-vs-gRPC-Web.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
