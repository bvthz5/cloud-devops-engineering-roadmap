# 02 - Amazon Route 53: The 7 DNS Routing Policies Explained

> **Focus:** Global DNS traffic management, Route 53 Hosted Zones, Alias Records, and comprehensive breakdown of the 7 Routing Policies.

---

## 🗺️ 1. The 7 Route 53 Routing Policies

Amazon Route 53 provides sophisticated DNS routing options to optimize network latency and provide automated failover:

| Routing Policy | Operating Principle | Real-World Enterprise Use Case |
| :--- | :--- | :--- |
| **Simple Routing** | Standard DNS resolution. Returns single or multiple IP addresses in random order. | Single server websites; basic domain lookups. |
| **Weighted Routing** | Distributes traffic proportionally based on assigned numerical weights (e.g., 80% to v1, 20% to v2). | Canary deployments, blue/green rollouts, A/B testing. |
| **Latency-Based Routing**| Routes users to the AWS Region that provides the lowest network latency from their location. | Global web portals distributed across multiple regions. |
| **Failover Routing** | Active-Passive configuration. Directs 100% traffic to primary; automatically fails over to backup on health check failure. | Mission-critical Disaster Recovery (DR). |
| **Geolocation Routing** | Routes traffic based on the geographic origin of the user's DNS query (Continent or Country). | Content localization, GDPR data compliance. |
| **Geoproximity Routing**| Routes traffic based on geographic location of resources and users, with customizable **Bias** adjustments. | Shifting traffic dynamically between regional datacenters. |
| **Multivalue Answer** | Returns up to 8 healthy IP addresses in response to DNS query with client-side load balancing. | DNS-level resilience without an ELB. |

---

## 🏷️ 2. Route 53 Alias Records vs. CNAME Records

- **CNAME Record:** Points a hostname to another hostname (e.g., `app.example.com` $ightarrow$ `lb.amazonaws.com`). Cannot be used for the Zone Apex (`example.com`).
- **Alias Record (AWS Proprietary):** Automatically resolves the **Zone Apex (`example.com`)** directly to AWS resources (ALB, CloudFront, S3 Bucket). AWS does not charge for DNS queries against Alias records!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Amazon Route53 DNS Architecture and Hosted Zones](./01-Amazon-Route53-DNS-Architecture-and-Hosted-Zones.md) | [Index](../../../README.md) | [03 - Route53 Health Checks and DNS Failover →](./03-Route53-Health-Checks-and-DNS-Failover.md) |
