# 07 - Real-World Scenarios & Outage Post-Mortems

## Outage Post-Mortem: The Single-Zone Catastrophe

### Incident Summary
During an AWS Availability Zone outage in `us-east-1a`, a company's web application went completely dark, despite having 12 running replicas and a multi-AZ cluster.

### Root Cause
No `topologySpreadConstraints` or `podAntiAffinity` rules were configured. By coincidence, the scheduler placed all 12 replicas on worker nodes located inside `us-east-1a`. When the AZ suffered a power failure, 100% of application capacity was destroyed.

### Remediation
Configured `topologySpreadConstraints` with `topologyKey: topology.kubernetes.io/zone` and `maxSkew: 1`. Replicas are now strictly distributed 4-4-4 across `us-east-1a`, `us-east-1b`, and `us-east-1c`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - PriorityClasses & Descheduler](./06-PriorityClasses-Preemption-and-the-Kubernetes-Descheduler.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
