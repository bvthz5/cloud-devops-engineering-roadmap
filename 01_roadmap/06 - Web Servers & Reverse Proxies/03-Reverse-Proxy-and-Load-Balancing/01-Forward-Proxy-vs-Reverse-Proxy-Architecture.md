# 01 - Forward Proxy vs Reverse Proxy Architecture

## 1. Architectural Distinctions

```text
Forward Proxy (Client-Facing / Outbound Egress):
[ Internal Clients ] ──► [ Forward Proxy (Squid) ] ──► (Public Internet) ──► [ External Servers ]
* Protects & anonymizes the CLIENT.
* Enforces corporate egress filtering, caching, and DLP.
* Client is configured to explicitly use the proxy.

Reverse Proxy (Server-Facing / Inbound Ingress):
[ External Clients ] ──► (Public Internet) ──► [ Reverse Proxy (Nginx) ] ──► [ Private Backend Fleet ]
* Protects & hides the SERVER infrastructure.
* Enforces load balancing, TLS termination, WAF, and caching.
* Client has zero awareness of internal backend topology.
```

---

## 2. Key Responsibilities of an Enterprise Reverse Proxy
1. **Load Balancing**: Distributing incoming traffic across multiple backend instances.
2. **TLS Offloading / Termination**: Decrypting TLS at the edge so backends process plaintext HTTP or lightweight internal TLS.
3. **Attack Surface Reduction**: Internal application servers sit in private subnets with zero public IP addresses.
4. **Static Asset Offloading & Caching**: Serving images, CSS, and JS directly from edge memory/disk without touching application backends.
5. **Request Normalization & Header Scrubbing**: Stripping illegal headers, enforcing HTTP standards, and preventing HTTP Request Smuggling.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (02-Apache-HTTP-Server)](../02-Apache-HTTP-Server/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Layer 4 vs Layer 7 Load Balancing →](./02-Layer-4-vs-Layer-7-Load-Balancing.md) |
