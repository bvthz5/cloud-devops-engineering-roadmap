# 05 - DDoS Mitigation, Rate Limiting, and WAF at the Edge

## 1. DDoS Attack Classifications

| Layer | Attack Type | Mechanism | Edge Mitigation |
|---|---|---|---|
| **Layer 3 / 4** | UDP Amplification, SYN Flood, ICMP Flood | Saturate network bandwidth with multi-Tbps junk traffic | BGP Anycast distribution, SYN cookies, BGP Flowspec drop rules |
| **Layer 7** | HTTP GET Flood, Slowloris, Cache-Busting Query Floods | Exhaust web server CPU/database threads with complex queries | Edge Rate Limiting, Managed WAF challenges (Cloudflare Turnstile, CAPTCHA) |

---

## 2. Web Application Firewall (WAF) at the Edge

By evaluating OWASP Top 10 rule sets at the edge:
- SQL injection (`' OR 1=1 --`) and Cross-Site Scripting (`<script>`) payloads are blocked at the edge PoP.
- Malicious traffic never reaches the customer's cloud VPC or Kubernetes clusters.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Edge Compute](./04-Edge-Compute-Cloudflare-Workers-Lambda-Edge-Fastly-Compute.md) | [README](./README.md) | [06 - Dynamic Acceleration](./06-Dynamic-Content-Acceleration-and-TCP-Optimization.md) |
