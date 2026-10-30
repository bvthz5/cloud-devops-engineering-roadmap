# 12 - Quick-Revision & Enterprise Cheat Sheet

## Helm CLI Commands

```bash
helm create <name>
helm lint ./<chart>
helm template <release> ./<chart> --debug
helm install <release> ./<chart> --atomic --timeout 5m
helm upgrade <release> ./<chart> -f custom-values.yaml
helm rollback <release> <revision>
helm history <release>
helm uninstall <release>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (14-Kubernetes-Networking-CNI-and-NetworkPolicies) →](../14-Kubernetes-Networking-CNI-and-NetworkPolicies/01-Kubernetes-Networking-Model-and-IP-per-Pod-Rule.md) |
