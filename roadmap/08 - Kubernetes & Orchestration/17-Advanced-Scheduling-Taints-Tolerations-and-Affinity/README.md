# 17 - Advanced Scheduling, Taints, Tolerations, and Affinity

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Kubernetes-Scheduler-Architecture-and-Scoring-Phases.md` — The scheduler framework: PreFilter, Filter, PreScore, Score, Reserve, and Bind phases.
2. `02-Node-Selection-NodeName-NodeSelector-and-NodeAffinity.md` — Direct node targeting vs soft/hard NodeAffinity (`requiredDuringScheduling` vs `preferredDuringScheduling`).
3. `03-Pod-Affinity-and-Pod-Anti-Affinity-Co-location-Rules.md` — Co-locating related services and anti-affinity rules to prevent single point of failure (SPOF) on nodes and racks.
4. `04-Taints-and-Tolerations-Node-Cordon-and-Drain-Mechanics.md` — Repelling pods: `NoSchedule`, `PreferNoSchedule`, `NoExecute`, node cordoning, and graceful eviction draining.
5. `05-Topology-Spread-Constraints-Multi-AZ-High-Availability.md` — Evenly distributing workloads across failure domains: `topologyKey`, `maxSkew`, and `whenUnsatisfiable`.
6. `06-PriorityClasses-Preemption-and-the-Kubernetes-Descheduler.md` — Workload importance: PriorityClass, evicting lower-priority pods (Preemption), and the Descheduler.
7. `07-Real-World-Scenarios.md` — Production post-mortems: all pods scheduled to a single AZ during cloud outage, and priority preemption cascade.
8. `08-Troubleshooting.md` — Diagnostic runbook for `0/N nodes available` scheduling failures.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Kubernetes scheduling.
10. `10-Hands-On-Practice.md` — Production lab: configuring multi-AZ TopologySpreadConstraints and dedicated GPU node taints.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with detailed explanations.
12. `12-Quick-Revision.md` — High-density scheduling syntax reference cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 16 - Troubleshooting](../16-Kubernetes-Troubleshooting-and-Debugging/README.md) | [README](./README.md) | [01 - Scheduler Architecture](./01-Kubernetes-Scheduler-Architecture-and-Scoring-Phases.md) |
