# 03 - Pods and Workloads

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Pod-Architecture-Pause-Container-and-Lifecycle.md` — The atomic unit of Kubernetes: pause container, shared network/IPC, and Pod phases (`Pending`, `Running`, `Succeeded`, `Failed`, `CrashLoopBackOff`).
2. `02-Liveness-Readiness-and-Startup-Probes.md` — Health checks: HTTP GET, TCP Socket, and Exec probes; initialDelaySeconds, periodSeconds, and failureThreshold tuning.
3. `03-Init-Containers-Sidecars-and-Ephemeral-Containers.md` — Specialized containers: sequential init containers, native 1.28+ sidecar containers, and ephemeral live debugging.
4. `04-Resource-Requests-Limits-and-Quality-of-Service-QoS.md` — CPU shares, CFS quota, memory limits, and QoS classes (`Guaranteed`, `Burstable`, `BestEffort`).
5. `05-Pod-Disruption-Budgets-PDB-and-Graceful-Termination.md` — High availability during node drains: `minAvailable`, `maxUnavailable`, SIGTERM vs SIGKILL, and `terminationGracePeriodSeconds`.
6. `06-Security-Contexts-RunAsUser-and-Privilege-Escalation.md` — Container isolation: `runAsNonRoot`, `readOnlyRootFilesystem`, dropping capabilities, and seccomp profiles.
7. `07-Real-World-Scenarios.md` — Production post-mortems: OOMKilled cascade during morning traffic, probe misconfigurations causing restart loops.
8. `08-Troubleshooting.md` — Diagnostic decision tree for Pod startup failures and probe timeouts.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Pod internals and lifecycle.
10. `10-Hands-On-Practice.md` — Production lab: deploying a multi-container pod with init container, probes, and resource constraints.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with collapsible answers.
12. `12-Quick-Revision.md` — High-density Pod specification cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 02 - Kubectl & Local Clusters](../02-Kubectl-and-Local-Clusters/README.md) | [README](./README.md) | [01 - Pod Architecture](./01-Pod-Architecture-Pause-Container-and-Lifecycle.md) |
