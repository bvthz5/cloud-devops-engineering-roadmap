# 01 - Pod Failure States: CrashLoopBackOff, OOMKilled, and ImagePullBackOff

## 1. The Pod Triage Matrix

```text
+---------------------+---------------------------------------------------------+-------------------------------------------------------+
| Status              | Underlying Cause                                        | Key Diagnostic Command                                |
+---------------------+---------------------------------------------------------+-------------------------------------------------------+
| **CrashLoopBackOff**| Application crashed on startup; restarting with delay.  | `kubectl logs <pod> --previous`                       |
| **OOMKilled (137)** | Process exceeded cgroup memory limit or node ran out.   | `kubectl describe pod <pod> | grep -i oom`            |
| **ImagePullBackOff**| Image tag not found, registry auth failed, or rate limit| `kubectl describe pod <pod> (Events)`                 |
| **CreateContainer** | Missing ConfigMap, Secret, or invalid volume mount.     | `kubectl describe pod <pod> (Events)`                 |
| **Pending**         | Scheduler cannot find a node satisfying predicates.     | `kubectl describe pod <pod> (Look for FailedScheduling|
+---------------------+---------------------------------------------------------+-------------------------------------------------------+
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (15-CRDs-and-Kubernetes-Operators)](../15-CRDs-and-Kubernetes-Operators/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Node NotReady Troubleshooting and Kubelet Diagnostics →](./02-Node-NotReady-Troubleshooting-and-Kubelet-Diagnostics.md) |
