# 09 - Interview Q&A: Cloud & Kubernetes

### Q: How does Ansible manage Kubernetes objects declaratively?
**Answer:** Using the `kubernetes.core.k8s` module which accepts inline YAML manifests or template files (`src: deployment.j2`) and applies them via K8s API.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
