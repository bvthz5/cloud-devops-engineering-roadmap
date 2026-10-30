# 09 - Interview Questions & Architectural Scenarios

### Q1: What are the architectural differences between Caddy and Nginx?
**Answer**: Caddy is written in Go, offering memory safety (immune to C buffer overflows), automatic HTTPS by default with ZeroSSL/Let's Encrypt failover, native HTTP/3 (QUIC) support, and a dynamic JSON REST administration API. Nginx is written in C, offers slightly lower memory footprints per idle worker, and has a larger legacy ecosystem, but requires manual Certbot integration and configuration file reloads.

### Q2: How does HTTP/3 in Caddy eliminate Head-of-Line blocking?
**Answer**: HTTP/3 operates over UDP using the QUIC protocol. In TCP (used by HTTP/1 and HTTP/2), a single dropped packet stalls all multiplexed streams. In QUIC, each stream is handled independently at the transport layer; packet loss on one stream does not pause or block data transmission on other active streams.

### Q3: Why is the `ask` endpoint mandatory when enabling On-Demand TLS in Caddy?
**Answer**: Without `ask`, an attacker can send HTTPS requests with arbitrary domain names pointed to your IP address. Caddy will attempt to issue a certificate for every domain, quickly exhausting Let's Encrypt certificate rate limits and filling disk storage with unwanted keys. The `ask` webhook verifies that the domain belongs to a registered customer before initiating certificate issuance.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
