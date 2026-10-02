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
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 17 - Advanced Scheduling](../17-Advanced-Scheduling-Taints-Tolerations-and-Affinity/README.md) |
