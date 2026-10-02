# 04 - Health Checks, Flapping, and Graceful Draining

## 1. Active vs. Passive Health Checks

- **Active Health Checks:** The load balancer sends synthetic probes (e.g., `GET /healthz`) periodically (every 5-10s). If the probe fails $N$ consecutive times, the node is marked down.
- **Passive Health Checks:** The load balancer monitors real client traffic. If a backend returns 5 consecutive HTTP 500 errors or TCP connection resets, the load balancer temporarily ejects the backend from the pool.

---

## 2. Preventing Health Check Flapping (Hysteresis)

**Flapping** occurs when an overloaded server temporarily passes a health check, receives a flood of traffic, crashes, fails the health check, drops traffic, recovers, and repeats in an infinite failure loop.

### Production Solution: Asymmetric Thresholds
Configure high thresholds for recovery and low thresholds for ejection:
- `Unhealthy Threshold`: **2 failures** (Remove node quickly!).
- `Healthy Threshold`: **5 consecutive successes** (Ensure node is fully warmed up before re-admitting).
- `Timeout`: **2 seconds** (Do not wait 30s for an unresponsive backend).

---

## 3. Connection Draining (Graceful Deregistration)

When a node is being terminated (autoscaling scale-in or rolling deployment):
1. Load balancer stops routing **NEW** requests to the terminating instance.
2. Load balancer allows **IN-FLIGHT** requests to complete until a configured timeout (e.g. `deregistration_delay = 30s`).
3. Once in-flight requests hit 0 or timeout expires, the node is safely shut down.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Load Balancing Algorithms Deep Dive](./03-Load-Balancing-Algorithms-Deep-Dive.md) | [Index](../../../README.md) | [05 - TLS SSL Termination Passthrough and mTLS →](./05-TLS-SSL-Termination-Passthrough-and-mTLS.md) |
