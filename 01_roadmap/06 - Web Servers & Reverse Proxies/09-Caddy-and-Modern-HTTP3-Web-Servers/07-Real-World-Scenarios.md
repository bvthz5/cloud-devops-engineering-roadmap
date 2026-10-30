# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The On-Demand TLS DDoS Outage

### Context & Incident
A SaaS startup enabled `on_demand` TLS in Caddy to issue certificates for white-label customer domains. An attacker launched a DDoS attack sending HTTPS handshakes with random domains (`a123.evil.com`, `b456.evil.com`). Caddy attempted to issue certificates for every domain, hitting Let's Encrypt's strict rate limits and locking out real customers for 7 days.

### Root Cause
`on_demand_tls` was configured without the mandatory **`ask`** authentication endpoint!

### Architectural Solution
Always protect On-Demand TLS with an `ask` verification webhook:
```caddyfile
{
    on_demand_tls {
        ask http://internal-billing-api:8080/is-registered-domain
    }
}
```
Caddy queries the backend first; if the domain is not in the customer database (HTTP 4xx), Caddy aborts the TLS handshake immediately without contacting Let's Encrypt.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Caddy as a Kubernetes Ingress and Container Edge](./06-Caddy-as-a-Kubernetes-Ingress-and-Container-Edge.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
