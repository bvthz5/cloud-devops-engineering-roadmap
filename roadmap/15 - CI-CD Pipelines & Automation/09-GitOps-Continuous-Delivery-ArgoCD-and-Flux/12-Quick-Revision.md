# Quick Revision Notes - GitOps & Continuous Delivery (ArgoCD & Flux)

> **Module**: GitOps & Continuous Delivery (ArgoCD & Flux)

---

## ⚡ Key Cheat Sheet

| DevSecOps / GitOps Topic | Core Function | Primary Tooling |
|---|---|---|
| **ArgoCD / Flux** | GitOps Continuous Delivery & Reconciliation | ArgoCD, Flux v2 |
| **Cosign / Sigstore** | Container image signing and verification | Cosign, Kyverno |
| **SBOM Generation** | Inventory of software dependencies | Syft, Trivy, CycloneDX |
| **OIDC Authentication** | Passwordless short-lived cloud auth | GitHub OIDC, AWS STS, GCP Workload Identity |
| **Feature Flags** | Decouple code deployment from release | LaunchDarkly, OpenFeature, Unleash |

---

## 📝 Top 5 Rules to Remember
1. **Adopt GitOps for Kubernetes CD** to keep cluster state synchronized with Git and eliminate admin credentials in CI pipelines.
2. **Sign container images with Cosign** and enforce verification with Kyverno before running in production.
3. **Generate SBOMs for all build artifacts** to enable instant vulnerability impact tracing.
4. **Use OIDC for passwordless authentication** to AWS, Azure, and GCP in CI runners.
5. **Decouple deployments from business releases using Feature Flags**.
