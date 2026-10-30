# MCQ - GitOps & Continuous Delivery (ArgoCD & Flux)

> **Module**: GitOps & Continuous Delivery (ArgoCD & Flux)

---

### Question 1
Which GitOps mechanism ensures that if an operator manually edits a live Kubernetes deployment using `kubectl edit`, the cluster state is automatically restored to match Git?
- [ ] A) Webhook Mirroring
- [x] B) Self-Healing (`selfHeal: true`)
- [ ] C) OIDC Token Refresh
- [ ] D) Rolling Update Policy

*Explanation: ArgoCD self-healing automatically detects manual cluster modifications and reconciles the state back to the Git source.*

---

### Question 2
What format standard is widely used for creating machine-readable Software Bill of Materials (SBOM)?
- [ ] A) YAML v1.2
- [x] B) CycloneDX / SPDX
- [ ] C) JSON Schema v4
- [ ] D) HCL2

*Explanation: CycloneDX and SPDX are the industry standards for generating Software Bill of Materials (SBOMs).*

---

### Question 3
What is the primary benefit of OpenID Connect (OIDC) authentication in CI/CD pipelines?
- [ ] A) It compresses Docker images before uploading to registries
- [x] B) It generates temporary short-lived credentials for cloud authentication, eliminating static API keys
- [ ] C) It speeds up unit test execution
- [ ] D) It automatically updates Helm chart dependencies

*Explanation: OIDC allows CI/CD runners to authenticate to AWS/Azure/GCP dynamically using short-lived tokens.*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
