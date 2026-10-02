# 16 - Kubernetes Troubleshooting and Debugging

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Pod-Failure-States-CrashLoopBackOff-OOMKilled-ImagePullBackOff.md` — Decoding pod failures: exit codes, previous container logs, termination messages, and cgroup OOM.
2. `02-Node-NotReady-Troubleshooting-and-Kubelet-Diagnostics.md` — Worker node forensics: kubelet systemd logs, PLEG timeouts, container runtime socket failures, and disk pressure.
3. `03-Network-Debugging-DNS-Resolution-Failures-and-Packet-Loss.md` — In-cluster networking triage: CoreDNS logs, conntrack table exhaustion, MTU black holes, and packet tracing with netshoot.
4. `04-Control-Plane-Diagnostics-API-Server-and-etcd-Failures.md` — Master node triage: API server connection refused, etcd quorum loss, leader election thrashing, and certificate expirations.
5. `05-Ephemeral-Debug-Containers-and-Kubectl-Debug.md` — Interactive live triage: `kubectl debug`, attaching ephemeral containers to distroless pods, and node filesystem chrooting.
6. `06-Cluster-Auditing-Event-Analysis-and-Forensics.md` — Forensic audit logs: Kubernetes API audit policy, filtering events via JSONPath, and SIEM security ingestion.
7. `07-Real-World-Scenarios.md` — Production post-mortems: the cascade PLEG node meltdown, and mysterious midnight pod terminations.
8. `08-Troubleshooting.md` — The ultimate Kubernetes diagnostic decision tree and triage runbook.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on high-pressure cluster troubleshooting.
10. `10-Hands-On-Practice.md` — Interactive lab: debugging a broken distroless pod using ephemeral debug containers and netshoot.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with detailed explanations.
12. `12-Quick-Revision.md` — High-density triage command reference and exit code cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 15 - CRDs & Operators](../15-CRDs-and-Kubernetes-Operators/README.md) | [README](./README.md) | [01 - Pod Failure States](./01-Pod-Failure-States-CrashLoopBackOff-OOMKilled-ImagePullBackOff.md) |
