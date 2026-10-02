# 11 - Auto-Scaling: HPA, VPA, and Cluster Autoscaler

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Horizontal-Pod-Autoscaler-HPA-v2-and-Metrics-APIs.md` — Pod horizontal elasticity: HPA v2 API, Metrics Server, CPU/Memory target utilization algorithms.
2. `02-Custom-and-External-Metrics-with-Prometheus-Adapter.md` — Advanced scaling triggers: Prometheus Adapter, Kafka consumer lag, HTTP request rate, and queue depth.
3. `03-Vertical-Pod-Autoscaler-VPA-Modes-and-Recommender.md` — Vertical right-sizing: VPA Recommender, Updater, and modes (`Off`, `Initial`, `Recreate`, `Auto`).
4. `04-Cluster-Autoscaler-Architecture-and-Cloud-Integration.md` — Node-level elasticity: Pending pod simulation, cloud provider node groups, and scale-down thresholds.
5. `05-Karpenter-Next-Generation-Just-in-Time-Node-Autoscaling.md` — High-speed node provisioning: Karpenter `NodePool`, direct cloud API calls, spot interruption handling, and bin-packing.
6. `06-Autoscaling-Anti-Patterns-Thrashing-and-Conflict-Resolution.md` — Production pitfalls: HPA/VPA conflicts on CPU/memory, flapping/thrashing, and stabilization windows.
7. `07-Real-World-Scenarios.md` — Production post-mortems: Black Friday cluster autoscaler API rate-limiting failure, and HPA flapping cascade.
8. `08-Troubleshooting.md` — Diagnostic runbook for `<unknown>` metric values in HPA and pending nodes in Cluster Autoscaler.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Kubernetes autoscaling architectures.
10. `10-Hands-On-Practice.md` — Production lab: deploying HPA v2 with stabilization windows, Apache Bench load testing, and scaling verification.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with detailed explanations.
12. `12-Quick-Revision.md` — High-density autoscaling algorithm and formula reference cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 10 - Jobs & CronJobs](../10-Jobs-and-CronJobs/README.md) | [README](./README.md) | [01 - HPA v2 Architecture](./01-Horizontal-Pod-Autoscaler-HPA-v2-and-Metrics-APIs.md) |
