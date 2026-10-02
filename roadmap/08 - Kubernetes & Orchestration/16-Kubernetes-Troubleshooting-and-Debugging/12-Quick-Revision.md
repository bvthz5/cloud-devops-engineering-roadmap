# 12 - Quick-Revision & Enterprise Cheat Sheet

## Troubleshooting Quick Reference

- **CrashLoopBackOff:** `kubectl logs <pod> --previous`.
- **Pending Pod:** `kubectl describe pod <pod>` (Events).
- **Distroless Triage:** `kubectl debug -it <pod> --image=nicolaka/netshoot`.
- **Node NotReady:** `sudo journalctl -u kubelet -e`.
- **DNS Issues:** Check CoreDNS pods and conntrack table saturation.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (17-Advanced-Scheduling-Taints-Tolerations-and-Affinity) →](../17-Advanced-Scheduling-Taints-Tolerations-and-Affinity/01-Kubernetes-Scheduler-Architecture-and-Scoring-Phases.md) |
