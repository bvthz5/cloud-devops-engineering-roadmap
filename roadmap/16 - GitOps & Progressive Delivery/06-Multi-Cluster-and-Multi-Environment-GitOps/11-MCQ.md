# MCQ - Multi-Cluster & Multi-Environment GitOps

> **Module**: Multi-Cluster & Multi-Environment GitOps

---

### Question 1
Which Kubernetes operator dynamically fetches secret values from AWS Secrets Manager or HashiCorp Vault and creates native Kubernetes Secret objects?
- [ ] A) Argo Rollouts
- [x] B) External Secrets Operator (ESO)
- [ ] C) Flagger
- [ ] D) Kustomize Patcher

*Explanation: ESO securely bridges cloud secret providers into native Kubernetes Secrets.*

---

### Question 2
Which ArgoCD feature allows deploying application manifests across 100+ clusters using dynamic Git or Cluster generators?
- [ ] A) Sync Hooks
- [x] B) ApplicationSets
- [ ] C) Health Probes
- [ ] D) Sealed Secrets

*Explanation: ApplicationSets generate multiple ArgoCD Application manifests dynamically using matrix/git/cluster generators.*

---

### Question 3
What open-source tool uses GPG or KMS keys to encrypt specific value fields inside YAML files before committing them to Git?
- [ ] A) Kaniko
- [ ] B) Helm Chart
- [x] C) SOPS (Secrets OPerationS)
- [ ] D) OpenTelemetry

*Explanation: SOPS selectively encrypts YAML/JSON value fields using KMS or GPG keys while leaving keys human-readable.*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
