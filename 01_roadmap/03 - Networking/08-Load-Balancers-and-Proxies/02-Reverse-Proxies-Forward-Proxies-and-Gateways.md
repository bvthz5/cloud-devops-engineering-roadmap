# 02 - Reverse Proxies, Forward Proxies, and API Gateways

## 1. What is a Forward Proxy?
A **Forward Proxy** sits in front of internal clients to manage, inspect, cache, or filter outbound internet traffic.
- **Enterprise Use Case:** Compliance, data loss prevention (DLP), caching internal software packages, URL filtering for corporate workstations.

```
[Internal Developer / EC2] ──► [Forward Proxy (Squid)] ──► [Public Internet (GitHub/Docker Hub)]
```

---

## 2. What is a Reverse Proxy?
A **Reverse Proxy** sits in front of backend servers to intercept incoming client requests and forward them to internal private microservices.
- **Enterprise Use Case:** SSL termination, load balancing, DDoS shielding, gzip compression, static asset caching.

```
[Public Internet User] ──► [Reverse Proxy (NGINX/Envoy)] ──► [Private App Server 10.0.1.5]
```

---

## 3. What is an API Gateway?
An **API Gateway** is an advanced Layer 7 reverse proxy tailored specifically for microservices architectures:
- **Authentication & Authorization:** Validates OAuth2 / JWT tokens at the perimeter.
- **Rate Limiting & Throttling:** Protects APIs using token bucket / leaky bucket algorithms.
- **Request Transformation:** Strips headers, converts JSON to gRPC payloads.
- **Service Discovery Integration:** Automatically dynamically routes traffic to pods discovered via Kubernetes or Consul.
- **Examples:** Kong, Traefik, Apache APISIX, AWS API Gateway.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Layer 4 vs Layer 7 Load Balancing](./01-Layer-4-vs-Layer-7-Load-Balancing.md) | [Index](../../../README.md) | [03 - Load Balancing Algorithms Deep Dive →](./03-Load-Balancing-Algorithms-Deep-Dive.md) |
