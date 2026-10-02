# 05 - TLS Termination, Passthrough, and Mutual TLS (mTLS)

## 1. TLS Deployment Architectures

```
1. TLS TERMINATION (Most Common)
   Client ──[ HTTPS (Encrypted) ]──► [ Load Balancer ] ──[ HTTP (Plaintext Private VPC) ]──► Backend

2. TLS RE-ENCRYPTION (End-to-End Compliance / PCI-DSS / HIPAA)
   Client ──[ HTTPS ]──► [ Load Balancer (Decrypts & Re-encrypts) ] ──[ HTTPS (Internal CA) ]──► Backend

3. TLS PASSTHROUGH (Zero-Knowledge / High Security)
   Client ──[ HTTPS (Encrypted) ]───────────────────────────────────────────────────────────► Backend
                              [ Load Balancer routes via SNI without decrypting ]
```

---

## 2. Mutual TLS (mTLS) in Service Meshes

In modern microservices (Istio, Linkerd, Consul):
- Every service pod contains an **Envoy sidecar proxy**.
- Both the client and the server validate each other's X.509 certificates.
- Guarantees zero-trust encryption and cryptographic service identity across internal networks.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Health Checks](./04-Health-Checks-Flapping-and-Graceful-Drain.md) | [README](./README.md) | [06 - Sticky Sessions](./06-Session-Persistence-and-Sticky-Sessions.md) |
