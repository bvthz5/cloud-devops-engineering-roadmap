# Troubleshooting Guide - GitOps & Continuous Delivery (ArgoCD & Flux)

> **Module**: GitOps & Continuous Delivery (ArgoCD & Flux)

---

## 🔍 Common Issue 1: ArgoCD Application OutOfSync / ComparisonError

### Symptom
ArgoCD shows an application status as `OutOfSync` or `ComparisonError` even though the Git repository has no pending uncommitted changes.

### Root Cause & Resolution
1. **Custom Resource Definition (CRD) Drift**: External controllers (e.g., HPA, Cert-Manager) modified runtime fields (like `status` or default annotations) not present in Git.
2. **Ignore Differences**: Add `ignoreDifferences` rules in the ArgoCD Application manifest:
   ```yaml
   spec:
     ignoreDifferences:
     - group: apps
       kind: Deployment
       jsonPointers:
       - /spec/replicas
   ```

---

## 🔍 Common Issue 2: OIDC Token Rejection (`Invalid Identity Token`)

### Symptom
A GitHub Actions pipeline step attempting to assume AWS IAM or GCP IAM roles fails with `AssumeRoleWithWebIdentity: Invalid identity token` or `OIDC provider mismatch`.

### Root Cause & Resolution
1. **Audience / Subject Mismatch**: The IAM role trust policy `sub` condition does not match the GitHub repository name, environment, or ref exact string.
2. **Audit Claims**: Inspect the OIDC JWT token claims (`aud: sts.amazonaws.com`, `sub: repo:org/repo:ref:refs/heads/main`).
3. **IAM Trust Policy Update**: Update the AWS/GCP IAM trust policy to match exact claim parameters.
