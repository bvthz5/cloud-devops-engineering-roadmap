# Module 14: Modern Web Protocols: HTTP/3, QUIC, and gRPC

Welcome to **Module 14: Modern Web Protocols: HTTP/3, QUIC, and gRPC**. The internet is undergoing its most radical transport transformation in 30 years: replacing TCP with QUIC over UDP, deploying HTTP/3, and standardizing inter-service microservice communications on gRPC with Protocol Buffers.

---

## 🎯 Learning Objectives

By the end of this module, you will be able to:
1. Understand why **TCP Ossification** and **Head-of-Line (HoL) Blocking** led to the creation of **QUIC over UDP**.
2. Master QUIC internals: **Integrated TLS 1.3**, **0-RTT connection handshakes**, and seamless **Connection Migration** across mobile networks.
3. Deconstruct **HTTP/3** frames and **QPACK** header compression.
4. Architect high-performance microservices using **gRPC**, **Protocol Buffers (Protobuf)**, and streaming patterns.
5. Choose precisely between **WebSockets**, **Server-Sent Events (SSE)**, and **gRPC-Web** for real-time applications.
6. Troubleshoot HTTP/3 fallbacks, `Alt-Svc` headers, and gRPC error status codes (`DEADLINE_EXCEEDED`, `UNAVAILABLE`).

---

## 📚 Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Evolution to QUIC UDP Transport](./01-Evolution-from-TCP-to-QUIC-UDP-Transport.md) | TCP ossification, middlebox interference, Head-of-Line blocking in TCP |
| 02 | [QUIC Protocol Internals & 0-RTT](./02-QUIC-Protocol-Internals-0-RTT-and-Connection-Migration.md) | Combined crypto transport handshake, Connection IDs, mobile WiFi-to-Cell handover |
| 03 | [HTTP/3 Architecture & QPACK](./03-HTTP3-Architecture-and-QPACK-Compression.md) | Elimination of HoL blocking across streams, QPACK dynamic tables |
| 04 | [gRPC & Protocol Buffers Architecture](./04-gRPC-Architecture-Protocol-Buffers-and-Streaming.md) | Binary serialization, Protobuf IDL, Unary and Bidirectional streaming |
| 05 | [Real-Time: WebSockets vs SSE vs gRPC](./05-Real-Time-Protocols-WebSockets-vs-SSE-vs-gRPC-Web.md) | Protocol selection matrix, architectural trade-offs, HTTP/2 streaming |
| 06 | [Observability & Debugging Tools](./06-Observability-and-Debugging-for-Modern-Protocols.md) | `grpcurl`, `grpcui`, `curl --http3`, analyzing Envoy gRPC access logs |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Mobile app connection loss solved via QUIC, microservice gRPC migration |
| 08 | [Troubleshooting Guide](./08-Troubleshooting.md) | Diagnosing corporate firewall UDP 443 drops, gRPC DEADLINE_EXCEEDED cascades |
| 09 | [Interview Questions & Answers](./09-Interview-QA.md) | 10 high-frequency production SRE/DevOps interview scenarios |
| 10 | [Hands-On Lab Practice](./10-Hands-On-Practice.md) | Compiling Protobuf definitions and testing gRPC endpoints with grpcurl |
| 11 | [Self-Assessment MCQs](./11-MCQ.md) | 10 scenario-based multiple-choice questions with deep explanations |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | Protocol comparison cheat sheet, gRPC status code table, QUIC flags |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [13 - CDN & Anycast](../13-CDN-Edge-Networks-and-Anycast-Routing/README.md) | [Networking Index](../README.md) | [01 - QUIC Transport](./01-Evolution-from-TCP-to-QUIC-UDP-Transport.md) |
