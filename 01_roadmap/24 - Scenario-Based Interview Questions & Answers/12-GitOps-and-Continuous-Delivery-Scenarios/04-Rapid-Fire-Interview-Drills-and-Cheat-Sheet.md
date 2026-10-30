# GitOps & Continuous Delivery Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: GitOps Rollback Failure Following Broken Production Release

### 🚨 The Production Scenario
A broken application release is deployed via GitOps. An on-call engineer attempts to rollback using `kubectl rollout undo deployment`, but within 60 seconds, the bad release is restored automatically, perpetuating the outage.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
GitOps uses Git as the single source of truth. When the engineer executed `kubectl rollout undo`, they altered the cluster state imperatively. ArgoCD detected cluster drift against Git and automatically self-healed, reapplying the broken manifest declared in Git.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- In GitOps, all rollbacks must be executed in Git, NOT via kubectl.
- Execute 'git revert HEAD' on the production deployment repository and push to main.
- ArgoCD detects the Git revert commit and synchronizes the previous stable state automatically.
- If immediate emergency rollback is required before Git CI finishes, disable auto-sync temporarily in ArgoCD, execute the imperative rollback, and immediately follow up with the Git revert.
- Conduct post-incident review emphasizing GitOps operating procedures to all on-call engineers.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Execute true GitOps rollback via Git commit revert
git revert HEAD --no-edit && git push origin main

# Trigger immediate synchronization to reverted stable commit
argocd app sync my-app

# Emergency break-glass: pause auto-sync if Git provider is unreachable
argocd app set my-app --sync-policy manual

# Inspect deployment history and commit hashes in ArgoCD
argocd app history my-app

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "In a GitOps architecture, 'kubectl rollout undo' is an anti-pattern that fails because the GitOps controller will immediately overwrite your manual change with what is in Git. A true GitOps rollback is executed via 'git revert'. Reverting the commit in Git provides an immutable audit trail, triggers the deployment automatically, and keeps Git and cluster state perfectly synchronized."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: ArgoCD OutOfSync CrashLoop Due to Mutating Webhook Modification** | `argocd app diff my-app --local .` | An in-cluster Mutating Admission Controller (e.g., Istio sidecar injector, Vault... |
| **Scenario 2: ArgoCD Sync Phase Failure - PreSync Database Migration Fails and Blocks Deployment** | `kubectl get jobs -l helm.sh/hook=pre-install,helm.sh/hook=pre-upgrade -A` | The migration script in the PreSync hook failed due to a syntax error or databas... |
| **Scenario 3: GitOps Secret Management - Plaintext Secret Committed to Git Repository** | `git filter-repo --invert-paths --path secrets/stripe-secret.yaml --force` | The developer lacked local pre-commit hooks and did not use GitOps secret manage... |
| **Scenario 4: Flagger Canary Rollout Halts & Rolls Back Due to Prometheus Metric Threshold Breaches** | `kubectl describe canary app-canary -n prod` | The canary container had a bug in a newly added API endpoint that threw 500 erro... |
| **Scenario 5: Flux v2 Kustomization Reconciliation Failure Due to Decryption Key Expiration** | `flux get kustomizations -A` | Mozilla SOPS was configured with a GPG key or age key whose cryptographic validi... |
| **Scenario 6: GitOps Monorepo Blast Radius - Shared Helm Subchart Upgrade Breaks 15 Services** | `argocd app set my-app --sync-policy manual` | All 15 applications referenced the shared Helm subchart via a loose version cons... |
| **Scenario 7: ArgoCD Repo Server Memory Exhaustion from Massive Helm/Kustomize Renders** | `kubectl top pod -l app.kubernetes.io/name=argocd-repo-server -n argocd` | The Git repository contained hundreds of Helm charts and massive Kustomize manif... |
| **Scenario 8: GitOps Drift Detection Alert Spam Caused by HorizontalPodAutoscaler (HPA)** | `kubectl get hpa my-app -n prod` | The Git repository declared `replicas: 3` in the Deployment manifest, but a Hori... |
| **Scenario 9: GitOps Multi-Cluster Sync Deadlock Due to Custom Resource Definition (CRD) Ordering** | `kubectl get crd \| grep cert-manager` | ArgoCD attempted to apply Custom Resources (CRs) in the same sync pass as the Cu... |
| **Scenario 10: GitOps Rollback Failure Following Broken Production Release** | `git revert HEAD --no-edit && git push origin main` | GitOps uses Git as the single source of truth. When the engineer executed `kubec... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← GitOps & Continuous Delivery Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Monitoring, Observability & Telemetry Scenarios: Production Incidents & Triage Scenarios →](../13-Monitoring-Observability-and-Telemetry-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

