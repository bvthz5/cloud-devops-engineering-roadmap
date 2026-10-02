# 06 — Rate Limiting Algorithms in API Gateways

Rate limiting protects backend systems from being overwhelmed by API abuse, credential stuffing, and runaway client retry loops.

---

## 1. The Core Rate Limiting Algorithms

### A. Token Bucket (Used by NGINX `limit_req`, AWS API Gateway)
- A bucket holds up to $B$ tokens (burst capacity).
- New tokens are added at a constant rate $r$ tokens/second.
- Each incoming request takes 1 token. If empty, the request is rejected with HTTP `429 Too Many Requests`.
- **Advantage:** Smoothly handles bursts of traffic up to bucket capacity while maintaining an average rate.

### B. Leaky Bucket
- Requests enter a queue.
- Requests leak out and are processed at a strictly constant rate.
- **Advantage:** Produces completely constant egress traffic; eliminates bursts.

### C. Sliding Window Counter
- Divides time into rolling windows. Computes an estimated request count by weighting the previous window with the current window.
- **Advantage:** Prevents double-traffic bursts at window boundaries that plague Fixed Window counters.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Trees, Graphs & DAGs](./05-Trees-and-Graph-Data-Structures.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
