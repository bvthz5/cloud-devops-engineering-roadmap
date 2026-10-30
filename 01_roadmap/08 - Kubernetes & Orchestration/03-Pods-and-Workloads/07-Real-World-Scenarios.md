# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The Dreaded Liveness Probe Blackout

### Incident Summary
A high-throughput e-commerce catalog service crashed completely across all 20 replicas during a promotional sale, resulting in 100% 502 Bad Gateway errors for 25 minutes.

### Root Cause
1. Traffic surged by 400%, causing application response latency to increase from 50ms to 2.2 seconds.
2. The `livenessProbe` was configured with:
   - `timeoutSeconds: 1`
   - `failureThreshold: 3`
3. Because the CPU was saturated serving incoming orders, the health check `/healthz` took 1.8 seconds to answer, exceeding the 1-second timeout.
4. Kubelet detected 3 consecutive timeouts and concluded the application was deadlocked.
5. Kubelet restarted all 20 pods simultaneously. During container boot, CPU spiked further, triggering immediate liveness timeouts on the newly started containers in an infinite restart loop!

### Remediation
- **Decouple Liveness from Traffic:** Liveness probes should ONLY verify internal process health (e.g., event loop responsive), never external database queries or full HTTP rendering.
- Set generous timeouts (`timeoutSeconds: 5`, `failureThreshold: 5`).
- Deploy a `startupProbe` to absorb initialization latency.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Security Contexts RunAsUser and Privilege Escalation](./06-Security-Contexts-RunAsUser-and-Privilege-Escalation.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
