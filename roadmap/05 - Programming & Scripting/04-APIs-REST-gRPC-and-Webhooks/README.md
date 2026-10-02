# Module 04: APIs, REST, gRPC, and Webhook Automation

Welcome to **Module 04: APIs, REST, gRPC, and Webhooks**. In modern cloud infrastructure, everything is controlled via APIs. Whether provisioning cloud resources, receiving GitHub triggers, or communicating across microservices, mastering APIs and webhooks is essential.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Deconstruct **REST API architecture**, HTTP verb semantics (PUT vs PATCH), and idempotency.
2. Differentiate between REST (JSON/HTTP/1.1) and **gRPC (Protocol Buffers/HTTP/2)** for high-throughput distributed systems.
3. Design and consume **Webhooks** with delivery guarantees (at-least-once delivery) and exponential backoff retries.
4. Verify webhook cryptographic integrity using **HMAC-SHA256 signatures** (preventing replay and spoofing attacks).
5. Implement authentication patterns: **API Keys**, **OAuth2 Client Credentials flow**, and **Mutual TLS (mTLS)**.
6. Build robust API client abstractions with rate-limiting, circuit breakers, and backoff jitter.

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [REST Architecture & Idempotency](./01-REST-API-Architecture-and-Idempotency.md) | Stateless design, HTTP methods (PUT vs PATCH), idempotent operations |
| 02 | [gRPC vs. REST for Microservices](./02-gRPC-vs-REST-Architectural-Comparison.md) | Binary serialization, protobuf IDL, streaming, inter-service communications |
| 03 | [Webhook Architecture & Delivery](./03-Webhook-Architecture-and-Reliable-Delivery.md) | Reverse APIs, at-least-once delivery, dead-letter queues, handling retry storms |
| 04 | [Webhook Security & HMAC Verification](./04-Webhook-Security-and-HMAC-SHA256-Signatures.md) | Cryptographic signature validation (`X-Hub-Signature-256`), replay prevention |
| 05 | [API Authentication Patterns](./05-API-Authentication-OAuth2-JWT-and-mTLS.md) | OAuth2 Client Credentials flow, token caching, mutual TLS verification |
| 06 | [Rate Limiting, Jitter & Backoff](./06-Rate-Limiting-Exponential-Backoff-and-Jitter.md) | Leaky bucket, token bucket, handling HTTP 429, full jitter algorithm |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Webhook retry storm collapsing payment gateway, non-idempotent billing duplicate |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Debugging signature verification failures, diagnosing truncated JSON responses |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps API interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Building a secure webhook receiver in Python with HMAC signature validation |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | HTTP status code summary, HMAC verification code snippet, backoff formula |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Golang](../03-Golang-Basics-for-Cloud-Native/README.md) | [README](./README.md) | [01 - REST Architecture](./01-REST-API-Architecture-and-Idempotency.md) |
