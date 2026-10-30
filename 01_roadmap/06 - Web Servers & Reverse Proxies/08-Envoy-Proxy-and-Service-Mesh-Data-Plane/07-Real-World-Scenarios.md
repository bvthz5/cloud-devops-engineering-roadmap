# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Control Plane Disconnect & Stale CDS Outage

### Context & Incident
A large Kubernetes cluster utilized an Envoy-based service mesh. The Istio control plane (`istiod`) crashed during a rolling upgrade. Immediately, applications began throwing HTTP 503 errors and dropping traffic between services.

### Root Cause
Envoy was configured with dynamic xDS, but the cluster warming timeout expired while the control plane was rebooting. Envoy purged un-revalidated clusters from memory, cutting off traffic to existing active pods.

### Architectural Solution
Configure **Static Cluster Fallbacks** and tune xDS reconnection backoff with `initial_fetch_timeout`:
```yaml
dynamic_resources:
  cds_config:
    initial_fetch_timeout: 0s # Never timeout waiting for control plane; retain existing state!
```
When `initial_fetch_timeout: 0s` is set, Envoy retains its in-memory cluster state indefinitely if the management server is unreachable.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Observability OpenTelemetry and Envoy Access Logs](./06-Observability-OpenTelemetry-and-Envoy-Access-Logs.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
