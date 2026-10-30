# GitOps & Continuous Delivery Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: Flagger Canary Rollout Halts & Rolls Back Due to Prometheus Metric Threshold Breaches

### 🚨 The Production Scenario
During an automated canary release via Flagger and Istio, Flagger initiates a 10% traffic split to the canary pod. After 3 minutes, Flagger detects 500 error rates exceeding 1%, aborts the rollout, and reverts traffic to the primary.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The canary container had a bug in a newly added API endpoint that threw 500 errors under specific input. Flagger queried Prometheus metric templates (http_requests_total) and detected error rates exceeding the defined metric analysis threshold (max-weight: 50%, threshold: 1%).

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect Flagger events and canary CRD status via kubectl describe canary.
- Review Prometheus metric queries defined in the MetricTemplate resource.
- Inspect canary pod logs during the rollout window to isolate the exact HTTP 500 exception.
- Patch the application code bug, write a regression test, and commit to Git.
- Monitor Flagger's progressive deployment cycle as it steps traffic from 10% to 50% to 100%.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check Flagger canary analysis events and rollback justification
kubectl describe canary app-canary -n prod

# Check current canary traffic split percentage
kubectl get canary app-canary -n prod -o jsonpath='{.status.canaryWeight}'

# Inspect canary container error logs
kubectl logs -l app=my-app-canary -n prod --tail=50

# Verify GitOps sync state reflects stable release
argocd app get my-app

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "This is progressive delivery working exactly as designed. Flagger shifted 10% of live traffic to the canary, evaluated real-time Prometheus error metrics against the SLI threshold, detected the breach, and autonomously restored 100% traffic to the primary deployment with zero human intervention and minimal blast radius. We inspect the canary pod logs, fix the bug in code, and re-deploy."

---

## 📌 Scenario 5: Flux v2 Kustomization Reconciliation Failure Due to Decryption Key Expiration

### 🚨 The Production Scenario
Flux v2 stops reconciling manifests across all clusters. The `flux get kustomizations` command reports: `Kustomization/prod-workloads reconciliation failed: error decrypting secret: GPG key expired`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Mozilla SOPS was configured with a GPG key or age key whose cryptographic validity expiration date was reached. When the Flux source controller pulled encrypted files, the kustomize-controller failed decryption, halting all cluster state reconciliation.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect Flux kustomization controller logs for the exact SOPS error.
- Generate an updated GPG key or transition to cloud KMS (AWS KMS, Azure Key Vault, or GCP Cloud KMS) to eliminate local key expiration risks.
- Update the SOPS configuration (.sops.yaml) and re-encrypt repository secrets with the new key.
- Update the decryption secret in the flux-system namespace.
- Trigger manual reconciliation with 'flux reconcile kustomization'.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# List status of all Flux Kustomization reconciliations
flux get kustomizations -A

# Stream Flux reconciliation errors
flux logs --level=error --kind=Kustomization

# Update SOPS decryption key in Flux namespace
kubectl create secret generic sops-gpg --namespace=flux-system --from-file=sops.asc=./new-key.asc --dry-run=client -o yaml | kubectl apply -f -

# Force immediate Flux re-reconciliation
flux reconcile kustomization prod-workloads --with-source

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Expiring GPG keys in SOPS creates ticking operational timebombs. To resolve the immediate outage, I update the GPG key secret in flux-system and trigger reconciliation. Architecturally, I migrate SOPS from GPG keys to Cloud KMS (AWS KMS or GCP KMS) using Workload Identity, eliminating manual key rotation and key expiration entirely."

---

## 📌 Scenario 6: GitOps Monorepo Blast Radius - Shared Helm Subchart Upgrade Breaks 15 Services

### 🚨 The Production Scenario
An engineer upgraded a shared library Helm subchart in a GitOps monorepo. Within 10 minutes, ArgoCD automatically synchronized and applied changes across 15 separate microservices, causing widespread outages.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
All 15 applications referenced the shared Helm subchart via a loose version constraint (`version: ">1.0.0"` or file path) rather than immutable pinned semantic versions, and the ArgoCD applications were configured with 'automated.selfHeal: true' and 'automated.prune: true' across all environments simultaneously.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Immediately disable ArgoCD auto-sync globally or on affected applications via CLI.
- Revert the Git commit that modified the shared Helm subchart.
- Enforce strict semantic versioning and immutable chart packaging: subcharts must be published to an OCI registry with strict version pinning.
- Implement multi-stage GitOps promotion: dev -> staging -> production branches or directory structures with automated PR gates.
- Re-enable auto-sync after confirming all 15 services recover.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Disable automated self-heal/sync during incident containment
argocd app set my-app --sync-policy manual

# Revert shared subchart change in Git
git revert <BREAKING_COMMIT_HASH> && git push origin main

# Synchronize all affected apps back to stable state
argocd app sync -l app.kubernetes.io/part-of=ecommerce --prune

# Package shared library with immutable semver
helm package ./charts/common-lib --version 1.2.1

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Automated GitOps in a monorepo without deployment gates turns any shared code error into an enterprise-wide outage. I mitigate this by treating shared Helm charts as immutable OCI packages with strict semantic versioning. Furthermore, we decouple environments so changes soak in staging before a GitOps promotion bot opens automated PRs to production."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← GitOps & Continuous Delivery Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [GitOps & Continuous Delivery Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

