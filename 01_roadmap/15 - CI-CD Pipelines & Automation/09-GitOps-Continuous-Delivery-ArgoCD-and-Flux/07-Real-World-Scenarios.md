# Real-World Scenarios - GitOps & Continuous Delivery (ArgoCD & Flux)

> **Module**: GitOps & Continuous Delivery (ArgoCD & Flux)

---

## 🏢 Scenario 1: Multi-Cluster GitOps Delivery with ArgoCD & ApplicationSets

### Background
An enterprise operates 30 Kubernetes clusters across AWS EKS and GCP GKE. Manually deploying Helm charts or updating kubectl manifests across clusters results in drift, misconfigurations, and delayed releases.

### Solution Architecture
1. **ArgoCD ApplicationSets**: Define an `ApplicationSet` custom resource targeting cluster labels (`env: production`, `cloud: aws`).
2. **Centralized Git Governance**: Application manifests are stored in a dedicated `k8s-infrastructure` repository.
3. **Automated Reconciliation**: ArgoCD continuously audits all 30 clusters, automatically syncing code changes pushed to `main` within 60 seconds and auto-healing configuration drift.

---

## 🏢 Scenario 2: Zero-Trust Software Supply Chain Security with Cosign & Kyverno

### Background
A healthcare cloud platform must guarantee that no unauthorized or modified container image can be executed on production GKE/EKS clusters, satisfying SOC2 and HIPAA compliance.

### Solution Architecture
1. **CI Signing**: In the GitHub Actions pipeline, after Trivy vulnerability scanning passes, container images are signed using `cosign` and Sigstore KMS key pairs.
2. **Policy-as-Code Enforcement**: Deploy **Kyverno** admission controller on production Kubernetes clusters with an `image-verify` policy.
3. **Admission Control**: Kyverno intercept pod creation requests; if an image signature is unverified or modified, Kubernetes rejects the deployment pod immediately.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Image Automation Automated Git Commits on Container Push](./06-Image-Automation-Automated-Git-Commits-on-Container-Push.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
