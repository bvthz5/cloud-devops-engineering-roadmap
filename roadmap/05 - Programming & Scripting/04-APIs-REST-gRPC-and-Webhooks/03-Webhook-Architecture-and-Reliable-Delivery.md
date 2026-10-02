# 03 - Webhook Architecture and Reliable Delivery

## 1. Polling vs. Webhooks (Reverse APIs)

- **Polling:** The client asks the server every 10 seconds: *"Has the build finished?"* (Consumes massive CPU and 99% of requests return empty).
- **Webhook (Push Event):** The server sends an HTTP `POST` request directly to the client's endpoint the exact millisecond the build finishes!

```
[ GitHub / Stripe / PagerDuty ] ──── HTTP POST (Event Payload) ────► [ DevOps Webhook Receiver ]
```

---

## 2. At-Least-Once Delivery & Retry Storms

Because networks are unreliable, webhook senders retry failed deliveries (status $4xx$ or $5xx$) with **Exponential Backoff**:
- **At-Least-Once Guarantee:** The webhook receiver may receive the **exact same event twice**!
- **Mandatory SRE Practice:** The receiver must check the event ID in Redis/database before processing to guarantee idempotency.
- **Fail Fast:** The receiver should respond `HTTP 200 OK` within 500ms, pushing the payload onto a queue (RabbitMQ / SQS / Redis) for background processing.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - gRPC vs REST Architectural Comparison](./02-gRPC-vs-REST-Architectural-Comparison.md) | [Index](../../../README.md) | [04 - Webhook Security and HMAC SHA256 Signatures →](./04-Webhook-Security-and-HMAC-SHA256-Signatures.md) |
