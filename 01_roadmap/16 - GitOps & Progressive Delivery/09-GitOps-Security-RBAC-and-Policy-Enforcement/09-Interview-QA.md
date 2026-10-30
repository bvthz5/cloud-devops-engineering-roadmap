# Interview Q&A - GitOps Security, RBAC & Policy Enforcement

> **Module**: GitOps Security, RBAC & Policy Enforcement

---

### Q1: What is the function of the External Secrets Operator (ESO) in a GitOps architecture?
**Answer**:
External Secrets Operator (ESO) bridges external secret management systems (like AWS Secrets Manager, HashiCorp Vault, Azure Key Vault, GCP Secret Manager) into Kubernetes. Instead of storing plaintext or encrypted secrets in Git, GitOps manifests store an `ExternalSecret` definition. ESO fetches secrets securely from the external provider and dynamically generates native Kubernetes `Secret` objects inside the cluster.

---

### Q2: How does Flagger execute automated progressive canary deployments?
**Answer**:
Flagger runs inside Kubernetes and watches custom `Canary` CRDs. When a new container image tag is detected, Flagger creates a canary deployment and manipulates service mesh / ingress routing (e.g., Istio, Linkerd, NGINX) to shift traffic incrementally (e.g., 5% -> 10% -> 50%). During traffic shifting, Flagger executes synthetic load tests (via k6/Helm) and queries Prometheus metrics; if error rates spike, Flagger automatically halts the rollout and restores 100% traffic to the stable version.

---

### Q3: How do ArgoCD ApplicationSets simplify multi-cluster deployments?
**Answer**:
Standard ArgoCD `Application` manifests require 1 file per cluster/application pair. **ApplicationSets** introduce templated generators (Git directory, Cluster list, Matrix) that automatically discover target clusters and repository paths, dynamically generating hundreds of ArgoCD `Application` instances from a single manifest.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
