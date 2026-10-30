# 09 - Interview Questions & Architectural Scenarios

### Q1: What problem does `ReferenceGrant` solve in Kubernetes Gateway API?
**Answer:**
`ReferenceGrant` enforces secure cross-namespace boundaries. In multi-tenant clusters, it prevents developers from unauthorized attachment of routes to gateways in other namespaces or accessing cross-namespace secrets/services without explicit permission from the namespace owner.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
