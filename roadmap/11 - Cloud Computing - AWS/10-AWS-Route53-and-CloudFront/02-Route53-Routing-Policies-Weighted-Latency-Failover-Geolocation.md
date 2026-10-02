# 02 - Route53 Routing Policies

- **Simple**: Standard A record mapping to single IP.
- **Weighted**: Split traffic based on percentage weights (e.g., 80% to v1, 20% to v2).
- **Latency-Based**: Direct users to the AWS region with lowest network latency.
- **Failover**: Active-Passive automated DNS failover based on health checks.
- **Geolocation**: Route based on user's physical geographic location.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Route53 Architecture](./01-Amazon-Route53-DNS-Architecture-and-Hosted-Zones.md) | [README](./README.md) | [03 - Health Checks & Failover](./03-Route53-Health-Checks-and-DNS-Failover.md) |
