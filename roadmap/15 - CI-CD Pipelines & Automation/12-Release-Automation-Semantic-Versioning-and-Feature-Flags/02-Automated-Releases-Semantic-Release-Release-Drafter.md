# Automated Releases: Semantic-Release & Release Drafter

> **Module**: Release Automation, Semantic Versioning & Feature Flags  
> **Level**: Production & Deep-Dive  

---

## 📌 Executive Summary
Mastering **Automated Releases: Semantic-Release & Release Drafter** is fundamental for building reliable, fast, and secure Continuous Integration and Continuous Deployment (CI/CD) pipelines. This guide covers pipeline mechanics, runner executor configurations, caching strategies, and enterprise deployment guardrails.

---

## 🏗 Architecture & Execution Flow

```
+-------------------------------------------------------------------------------+
|                        MODERN CI/CD AUTOMATION PIPELINE                      |
+-------------------------------------------------------------------------------+
|  Git Push / PR  -->  Trigger & Webhook  -->  Runner Allocation (K8s/VM)     |
|  Build & Test   -->  SAST/SCA Scan     -->  Artifact Publish  --> Deploy     |
+-------------------------------------------------------------------------------+
```

### Key Concepts
1. **GitOps Continuous Delivery**: Pull-based deployment model using ArgoCD or Flux to automatically reconcile live cluster state with Git.
2. **Software Supply Chain Security**: Generating Software Bill of Materials (SBOM), scanning third-party dependencies, and signing container images with Cosign.
3. **Passwordless Authentication (OIDC)**: Exchanging short-lived OIDC tokens between CI runners and AWS/Azure/GCP instead of using static secret keys.
4. **Decoupled Releases via Feature Flags**: Using LaunchDarkly or OpenFeature to toggle production feature visibility independently of code deployments.

---

## 🛠 Manifests & Configuration Examples

### ArgoCD Application Manifest (GitOps)
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: payments-service
  namespace: argocd
spec:
  project: default
  source:
    repoURL: 'https://github.com/org/k8s-manifests.git'
    targetRevision: HEAD
    path: environments/production
  destination:
    server: 'https://kubernetes.default.svc'
    namespace: payments
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
```

### Container Image Signing with Cosign
```bash
# Generate key pair
cosign generate-key-pair

# Sign container image in CI pipeline
cosign sign --key cosign.key us-central1-docker.pkg.dev/my-project/app:v1.2.0

# Verify signature at Kubernetes deploy time
cosign verify --key cosign.pub us-central1-docker.pkg.dev/my-project/app:v1.2.0
```

---

## 💡 Production Best Practices
- **Enforce Self-Healing GitOps**: Enable `selfHeal: true` and `prune: true` in ArgoCD so manual kubectl changes in production are immediately overridden by Git state.
- **Generate SBOMs on Every Build**: Produce CycloneDX or SPDX format SBOMs during CI build jobs to maintain transparency over open-source vulnerability risks.
- **Adopt Passwordless OIDC Tokens**: Replace permanent IAM access keys stored in GitHub Secrets with dynamic OIDC role assumption.
- **Use Semantic Release with Conventional Commits**: Automate version tagging (`v1.2.3`) and changelog creation based on commit prefixes (`feat:`, `fix:`, `breaking:`).

---

## 🔗 Related Resources
- [ArgoCD Documentation](https://argo-cd.readthedocs.io/)
- [Sigstore / Cosign Documentation](https://docs.sigstore.dev/cosign/overview/)
- [OpenFeature Standard](https://openfeature.dev/)
