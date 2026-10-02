# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the difference between Ingress and Gateway API?
**Answer:**
Ingress is a single monolithic resource with limited expressive power; advanced features (rewrites, canary, rate limiting) rely on vendor-specific annotations that cannot be ported across controllers.
Gateway API is an expressive, role-oriented, and extensible standard (GA in k8s 1.25+) that splits responsibilities across cluster operators (`GatewayClass`, `Gateway`) and application developers (`HTTPRoute`, `GRPCRoute`), providing native support for canary traffic splitting, header matching, and cross-namespace routing without annotations.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
