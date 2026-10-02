# 09 - GitOps: Interview Questions & Answers

### Q1: What is the core security advantage of Pull-Based GitOps over Push-Based CI/CD?
**Answer:** In push-based CI/CD, the external CI runner (e.g. GitHub Actions) must be granted long-lived, high-privilege administrative credentials to access and modify the production cluster API. In pull-based GitOps, the controller runs entirely inside the cluster network perimeter and pulls manifests unidirectionally. No inbound ports are opened, and no credentials leave the cluster.

### Q2: How should sensitive credentials (passwords, tokens) be managed in a GitOps repository?
**Answer:** Secrets must never be stored as plaintext base64 in Git. Best practice architectures use either **Mozilla SOPS** (encrypting values using cloud KMS keys before committing) or the **External Secrets Operator (ESO)**, which leaves secrets in AWS Secrets Manager / HashiCorp Vault and dynamically mounts them in Kubernetes.

### Q3: What happens when an engineer modifies a Kubernetes resource manually using `kubectl` in an automated GitOps environment?
**Answer:** If `selfHeal: true` is enabled on the GitOps controller (Argo CD / Flux), the controller detects drift between live cluster state and the Git repository. Within seconds, it overwrites the manual changes with the state declared in Git.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
