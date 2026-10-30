# 12 - Quick-Revision & Enterprise Cheat Sheet

## Essential Kubectl Commands

```bash
# Contexts & Namespaces
kubectl config get-contexts
kubectl config use-context <name>
kubectl config set-context --current --namespace=<ns>

# Quick Debugging
kubectl get events --sort-by='.metadata.creationTimestamp' -A
kubectl run debug --rm -it --image=busybox:musl -- sh
kubectl top nodes && kubectl top pods -A

# Explain Resources
kubectl explain pod.spec.containers.resources
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (03-Pods-and-Workloads) →](../03-Pods-and-Workloads/01-Pod-Architecture-Pause-Container-and-Lifecycle.md) |
