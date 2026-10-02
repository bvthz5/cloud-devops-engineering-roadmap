# 04 - Deployments, ReplicaSets, and Rollouts

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Deployment-Controller-and-ReplicaSet-Reconciliation.md` — The declarative orchestration hierarchy: Deployment ➔ ReplicaSet ➔ Pods.
2. `02-Rolling-Update-Strategy-MaxSurge-and-MaxUnavailable.md` — Zero-downtime updates: mathematical tuning of `maxSurge` and `maxUnavailable` (percentage vs absolute).
3. `03-Rollbacks-Revision-History-and-Change-Cause.md` — Managing release history: `revisionHistoryLimit`, `rollout undo`, and recording rollout annotations.
4. `04-Blue-Green-and-Canary-Deployments-Native-Patterns.md` — Advanced deployment topologies using native Service label selectors and traffic shifting.
5. `05-Pausing-Resuming-and-Scaling-Deployments.md` — Batch updates without triggering intermediate rollouts, and manual/declarative horizontal scaling.
6. `06-Container-Lifecycle-Hooks-PostStart-and-PreStop.md` — In-flight request draining: `preStop` sleep patterns and race condition defense.
7. `07-Real-World-Scenarios.md` — Production post-mortems: 502 Bad Gateway bursts during rollouts, and stuck deployment deadlocks.
8. `08-Troubleshooting.md` — Diagnostic runbook for rollout timeouts (`progressDeadlineSeconds`) and image pull failures.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Deployment rollout mechanics.
10. `10-Hands-On-Practice.md` — Zero-downtime rolling update lab with active load testing and instant rollback.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with collapsible explanations.
12. `12-Quick-Revision.md` — High-density rollout cheat sheet and command reference.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 03 - Pods & Workloads](../03-Pods-and-Workloads/README.md) | [README](./README.md) | [01 - Deployment Controller](./01-Deployment-Controller-and-ReplicaSet-Reconciliation.md) |
