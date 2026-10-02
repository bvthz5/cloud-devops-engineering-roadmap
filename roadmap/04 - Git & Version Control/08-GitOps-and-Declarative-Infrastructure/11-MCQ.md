# 11 - GitOps: Self-Assessment MCQs

### Q1. Which OpenGitOps principle states that an in-cluster software agent must periodically compare desired state with live state?
- A) Declarative Specification
- B) Versioned and Immutable
- C) Continuous State Reconciliation
- D) Zero Trust Networking
<details><summary><b>View Answer</b></summary><b>Correct Answer: C</b><br>Continuous Reconciliation ensures that any out-of-band cluster drift is detected and corrected.</details>

---

### Q2. Which Kubernetes operator reads credentials directly from AWS Secrets Manager or HashiCorp Vault to create in-memory K8s Secrets?
- A) Sealed Secrets
- B) External Secrets Operator (ESO)
- C) Kustomize SecretGenerator
- D) Argo CD Vault Plugin
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>External Secrets Operator integrates directly with external enterprise secret managers.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
