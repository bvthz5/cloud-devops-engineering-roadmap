# GitOps & Continuous Delivery Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: ArgoCD OutOfSync CrashLoop Due to Mutating Webhook Modification

### 🚨 The Production Scenario
An ArgoCD Application is stuck in an endless sync loop. ArgoCD reports 'OutOfSync' immediately after synchronizing, continuously reapplying resources every 3 minutes.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
An in-cluster Mutating Admission Controller (e.g., Istio sidecar injector, Vault agent injector, or an internal policy engine) altered the Pod specification (adding annotations, sidecar containers, or volumes). Because the Git repository did not declare these injected fields, ArgoCD detected a diff against Git and attempted to overwrite the cluster state.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Use 'argocd app diff' to identify the exact field causing the difference between Git and live cluster state.
- Configure 'ignoreDifferences' in the ArgoCD Application manifest targeting the specific mutating JSON path (e.g., metadata.annotations, spec.template.spec.containers).
- Alternatively, update the Helm chart or Kustomize base in Git to match the mutated fields natively.
- Verify sync status returns to 'Synced' and 'Healthy'.
- Document mutating webhook behaviors to prevent repeating diff collisions across teams.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect precise diff between Git manifests and live cluster state
argocd app diff my-app --local .

# Identify active mutating admission webhooks modifying pods
kubectl get mutatingwebhookconfigurations -o wide

# Check ArgoCD sync status and health report
argocd app get my-app

# Force synchronization test after configuring ignoreDifferences
argocd app sync my-app --force

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "ArgoCD endless sync loops happen when in-cluster mutating webhooks alter pod specs that Git does not know about. I run 'argocd app diff' to pinpoint the injected fields—frequently Istio sidecars or Vault volumes. I resolve this cleanly by declaring an 'ignoreDifferences' block in the ArgoCD Application spec for those specific JSON pointers, allowing the webhook to operate without breaking GitOps drift detection."

---

## 📌 Scenario 2: ArgoCD Sync Phase Failure - PreSync Database Migration Fails and Blocks Deployment

### 🚨 The Production Scenario
An ArgoCD deployment freezes in 'Progressing' state. Application pods are not updated because a PreSync Hook Job running database migrations failed with exit code 1.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The migration script in the PreSync hook failed due to a syntax error or database connection timeout. Because ArgoCD sync waves require PreSync hooks to terminate with exit code 0 before proceeding to the Main Sync phase, the entire deployment was blocked to protect application stability.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect the failed hook job pod logs using kubectl logs -l app.kubernetes.io/component=job.
- Examine the database error message and fix the migration script in Git.
- If the failed hook job persists, delete the failed job pod or configure 'hook-delete-policy: BeforeHookCreation,HookFailed' on the Job manifest.
- Commit the corrected migration script to Git.
- Trigger an ArgoCD sync to re-execute the migration hook and complete application rollout.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# List failed sync hook jobs in cluster
kubectl get jobs -l helm.sh/hook=pre-install,helm.sh/hook=pre-upgrade -A

# Inspect database migration failure stack trace
kubectl logs job/db-migration-job-v2 -n prod

# Clean up hung pre-sync hook job blocking ArgoCD
kubectl delete job db-migration-job-v2 -n prod

# Re-trigger sync wave deployment
argocd app sync my-app --prune

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "ArgoCD sync hooks are designed specifically to halt rollouts when pre-requisites fail. When a PreSync migration job crashes, ArgoCD rightfully freezes the sync wave to prevent deploying code against an unmigrated schema. I extract the failure from the job's pod logs, resolve the migration issue in Git, ensure the hook has 'HookFailed' delete policies configured so stale jobs don't block retries, and let GitOps drive the remediation."

---

## 📌 Scenario 3: GitOps Secret Management - Plaintext Secret Committed to Git Repository

### 🚨 The Production Scenario
A junior engineer accidentally committed a plaintext Kubernetes Secret containing a production Stripe API key directly to a public/shared GitOps repository.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The developer lacked local pre-commit hooks and did not use GitOps secret management tooling like Sealed Secrets, External Secrets Operator (ESO), or Mozilla SOPS.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Immediately revoke and rotate the compromised Stripe API key in the Stripe dashboard.
- Purge the secret from Git history across all branches and commits using 'git filter-repo' or BFG Repo-Cleaner.
- Force-push cleaned Git history and invalidate all cached CI/CD clones.
- Deploy External Secrets Operator (ESO) in Kubernetes to pull secrets dynamically from AWS Secrets Manager / Azure Key Vault / HashiCorp Vault.
- Install 'git-secrets' and 'trufflehog' pre-commit hooks to block future plaintext secret commits.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Purge committed secret file permanently from Git history
git filter-repo --invert-paths --path secrets/stripe-secret.yaml --force

# Scan repository history for leaked API keys and tokens
trufflehog git file://. --since-commit HEAD~5

# Verify External Secrets Operator is active
kubectl get externalsecrets.external-secrets.io -A

# Inspect backend vault connectivity for SecretStore
kubectl get secretstore -n prod -o yaml

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Step zero is immediate revocation and rotation of the credential at the provider level, because once a secret touches Git, it is considered compromised. Then I rewrite Git history with git-filter-repo to clean the repo. Long term, we ban Kubernetes Secret manifests in Git entirely, standardizing on External Secrets Operator (ESO) which synchronizes secrets from AWS Secrets Manager into cluster memory at runtime."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← CI/CD Pipelines & Automation Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../11-CICD-Pipelines-and-Automation-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [GitOps & Continuous Delivery Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

