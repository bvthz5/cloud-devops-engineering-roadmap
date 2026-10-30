# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Silent CPU Throttling Latency Outage

### Context & Incident
A payment microservice in Kubernetes experienced random p99 latency spikes of up to 4,000ms. CPU utilization reported across the fleet was only 35%.

### Root Cause
The container had a CPU limit configured: `limits: { cpu: "500m" }`. Linux CFS (Completely Fair Scheduler) enforces CPU limits across **100ms periods**. The multi-threaded application consumed its 50ms quota within the first 10ms of the period, causing the kernel to **throttle and freeze all application threads for the remaining 90ms of every single period**!

### Solution: Monitor `nr_throttled`
Remove CPU limits on latency-critical services or configure CPU limit-less nodes, relying on CPU requests and HPA for autoscaling.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - eBPF Based Container Tracing and Security Auditing](./06-eBPF-Based-Container-Tracing-and-Security-Auditing.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
