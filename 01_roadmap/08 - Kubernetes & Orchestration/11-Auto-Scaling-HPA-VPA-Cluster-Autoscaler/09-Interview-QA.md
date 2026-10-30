# 09 - Interview Questions & Architectural Scenarios

### Q1: What happens if you run HPA and VPA on the same Deployment for CPU?
**Answer:**
They will fight in a conflicting feedback loop:
1. High CPU load triggers HPA to add more Pod replicas.
2. Simultaneously, VPA observes high CPU and restarts Pods with larger CPU limits.
3. The new larger pods cause average CPU utilization to plummet.
4. HPA scales replicas down, causing CPU per pod to spike again.
**Rule:** Never use HPA and VPA on the same resource metric for the same workload.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
