# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the purpose of `externalTrafficPolicy: Local` on a Service?
**Answer:**
By default (`externalTrafficPolicy: Cluster`), when traffic hits a worker node's NodePort, `kube-proxy` may route it to a Pod on a different node, incurring an extra network hop and masking the client's source IP address with the node's IP (SNAT).
Setting `externalTrafficPolicy: Local`:
1. Preserves the client's real source IP.
2. Drops or rejects traffic if the node does not host an active replica of that Pod, eliminating the extra cross-node hop.

---

### Q2: What is the difference between `port`, `targetPort`, and `nodePort`?
**Answer:**
- `port`: The port exposed by the Service inside the cluster (what other pods send traffic to).
- `targetPort`: The port on which the containerized application is actually listening.
- `nodePort`: The port opened on every physical worker node interface (range 30000–32767) for external traffic.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
