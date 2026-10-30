# 12 - Quick-Revision & Enterprise Cheat Sheet

## Deployment Commands Quick Reference

```bash
kubectl rollout status deployment/<name>
kubectl rollout history deployment/<name>
kubectl rollout undo deployment/<name> [--to-revision=N]
kubectl rollout restart deployment/<name>
kubectl scale deployment/<name> --replicas=N
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (05-Services-and-Service-Discovery) →](../05-Services-and-Service-Discovery/01-Kubernetes-Service-Abstraction-and-Virtual-IPs.md) |
