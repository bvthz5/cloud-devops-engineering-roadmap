# 04 - SSL/TLS Certificates and HTTPS

Transport Layer Security (TLS) is the bedrock of secure internet communication. In modern Cloud & DevOps engineering, managing TLS encryption, automated certificate lifecycle management (ACME/Let's Encrypt), zero-trust Mutual TLS (mTLS), and high-performance cryptographic tuning are non-negotiable competencies.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [TLS Handshake Architecture: 1.2 vs 1.3](./01-TLS-Handshake-Architecture-1.2-vs-1.3.md) | Asymmetric vs symmetric crypto, ECDHE key exchange, 1-RTT vs 0-RTT, Forward Secrecy. |
| 02 | [Certificate Authorities, Chains & PKI](./02-Certificate-Authorities-Chains-and-Trust-Stores.md) | Root CA, Intermediate CA, Leaf certificate, SAN, PKI trust stores, path validation. |
| 03 | [Automated Certificates with ACME & Certbot](./03-Automated-Certificates-with-ACME-and-Certbot.md) | ACME v2 protocol, HTTP-01 vs DNS-01 challenges, Certbot CLI, systemd auto-renewal. |
| 04 | [High-Performance TLS Tuning & Session Resumption](./04-High-Performance-TLS-Tuning-and-Session-Resumption.md) | Session IDs vs Session Tickets, TLS 1.3 0-RTT, OCSP Stapling, ALPN HTTP/2 negotiation. |
| 05 | [Mutual TLS (mTLS) & Zero-Trust Architecture](./05-Mutual-TLS-mTLS-Architecture-and-Zero-Trust.md) | Client certificate authentication, `ssl_verify_client`, validating Subject DN in Nginx. |
| 06 | [SSL Labs A+ Hardening & Cipher Suites](./06-SSL-Labs-A-Plus-Configuration-and-Cipher-Suites.md) | Modern Mozilla TLS guidelines, HSTS preload, forward secrecy ciphers, DH parameters. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Expired wildcard certificate outage, missing intermediate chain. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | `openssl s_client`, checking expiry date via bash, verifying OCSP response, handshake debug. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior SRE/DevOps interview scenarios on TLS latency, mTLS, and certificate management. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Self-signed CA and mTLS setup; Lab 2: Nginx SSL Labs A+ server; Lab 3: ACME DNS-01. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for OpenSSL commands, Nginx TLS directives, and ACME flags. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Reverse Proxy & Load Balancing](../03-Reverse-Proxy-and-Load-Balancing/README.md) | [README](./README.md) | [01 - TLS Handshake Architecture](./01-TLS-Handshake-Architecture-1.2-vs-1.3.md) |
