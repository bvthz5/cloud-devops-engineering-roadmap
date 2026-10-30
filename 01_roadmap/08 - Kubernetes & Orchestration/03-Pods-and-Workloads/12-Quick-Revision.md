# 12 - Quick-Revision & Enterprise Cheat Sheet

## Pod Lifecycle Summary

- **Probe Types:** Startup (boot safety) ➔ Readiness (traffic routing) ➔ Liveness (deadlock recovery).
- **Exit Code 137:** OOMKilled (exceeded memory limit).
- **Exit Code 143:** Graceful shutdown via SIGTERM.
- **QoS Hierarchy:** Guaranteed (req == limit) > Burstable (req != limit) > BestEffort (no req/limit).
- **PreStop Hook:** Runs before SIGTERM; critical for draining in-flight requests.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (04-Deployments-ReplicaSets-and-Rollouts) →](../04-Deployments-ReplicaSets-and-Rollouts/01-Deployment-Controller-and-ReplicaSet-Reconciliation.md) |
