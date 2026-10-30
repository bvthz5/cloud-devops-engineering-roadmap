# GitOps & Continuous Delivery Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: ArgoCD Repo Server Memory Exhaustion from Massive Helm/Kustomize Renders

### 🚨 The Production Scenario
The ArgoCD web UI displays 'RPC Error: context deadline exceeded' when loading application details. The `argocd-repo-server` pod is trapped in CrashLoopBackOff with OOMKilled (exit code 137).

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The Git repository contained hundreds of Helm charts and massive Kustomize manifests. Every time the repo-server evaluated Git commits, it ran parallel manifest generation processes without memory limits, exhausting pod memory allocation.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Scale up the resource memory limits and requests on the argocd-repo-server deployment.
- Scale the argocd-repo-server horizontally to distribute rendering workloads across multiple replicas.
- Configure repository caching settings: increase repository cache TTL to reduce redundant manifest compilations.
- Split large monorepos into smaller, domain-specific repositories if practical.
- Optimize Helm/Kustomize templates to reduce excessive YAML boilerplate and deep nesting.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check real-time CPU/memory utilization of repo-server pods
kubectl top pod -l app.kubernetes.io/name=argocd-repo-server -n argocd

# Horizontally scale repo-server to distribute rendering load
kubectl scale deployment argocd-repo-server -n argocd --replicas=4

# Increase memory limit to eliminate OOMKilled crashes
kubectl set resources deployment argocd-repo-server -n argocd --limits=memory=4Gi,cpu=2000m

# Inspect caching and concurrency parameters
argocd-util repo-server --help

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "The argocd-repo-server is the computational workhorse of ArgoCD that compiles templates into raw YAML. When repos scale, default 1GB memory limits lead to OOMKilled crashes. I scale the repo-server deployment horizontally, increase memory limits to 4GB, and configure Redis caching for compiled manifests to drastically minimize redundant fork-exec render cycles."

---

## 📌 Scenario 8: GitOps Drift Detection Alert Spam Caused by HorizontalPodAutoscaler (HPA)

### 🚨 The Production Scenario
Security and platform teams receive thousands of Slack alerts daily for 'Cluster Drift Detected' on Deployment replica counts. Engineers begin ignoring drift alerts.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The Git repository declared `replicas: 3` in the Deployment manifest, but a Horizontal Pod Autoscaler (HPA) was scaling the deployment between 3 and 30 replicas based on CPU demand. Every time HPA changed the replica count, GitOps drift detection flagged the difference against Git.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Remove the explicit `replicas` field from the Kubernetes Deployment manifest in Git.
- Allow HPA to manage the replica count exclusively without Git interference.
- Configure ArgoCD or Flux ignoreDifferences for `/spec/replicas` on Deployments managed by HPA.
- Verify that HPA maintains autoscaling authority without triggering drift alerts.
- Update linting rules to flag manifests that define both HPA and hardcoded replicas.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check active HPA status and current replica targets
kubectl get hpa my-app -n prod

# Inspect live deployment replica count
kubectl get deployment my-app -n prod -o jsonpath='{.spec.replicas}'

# Verify ignored diff configuration for spec.replicas
argocd app get my-app --show-params

# Verify removal of hardcoded replica count in Git
git diff HEAD~1 manifests/deployment.yaml

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Having both Git declare replica counts and HPA scale pods in the cluster creates an architectural contradiction. The standard GitOps best practice is to omit 'spec.replicas' from the Deployment YAML in Git entirely, letting HPA own scaling. Additionally, we add an ignoreDifferences rule on 'spec.replicas' in ArgoCD so autoscaling never triggers false-positive drift alerts."

---

## 📌 Scenario 9: GitOps Multi-Cluster Sync Deadlock Due to Custom Resource Definition (CRD) Ordering

### 🚨 The Production Scenario
Deploying a new platform stack (Cert-Manager, Prometheus-Operator, Istio) across 20 clusters via GitOps fails. Applications fail to deploy with 'the server could not find the requested resource (Certificate, ServiceMonitor, VirtualService)'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
ArgoCD attempted to apply Custom Resources (CRs) in the same sync pass as the Custom Resource Definitions (CRDs). The Kubernetes API server had not finished registering and serving the CRD schemas when the client submitted the custom resources.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Separate CRDs into a dedicated GitOps application deployed in Sync Wave -1 or wave 0.
- Deploy the controllers/operators in Sync Wave 1.
- Deploy the Custom Resources (Certificates, ServiceMonitors, Ingresses) in Sync Wave 2 or later.
- Configure 'argocd.argoproj.io/sync-wave' annotations on manifests to enforce deterministic ordering.
- Verify that CRD establishment is confirmed before custom resource ingestion.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check whether required CRDs are registered in Kubernetes API
kubectl get crd | grep cert-manager

# Wait until CRD is fully established in API server
kubectl wait --for=condition=Established crd/certificates.cert-manager.io --timeout=60s

# Sync CRD wave first
argocd app sync platform-stack --sync-wave 0

# Audit sync-wave annotations across manifests
argocd app get platform-stack -o yaml | grep sync-wave

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "The Kubernetes API server cannot accept custom resources until the underlying CRD is registered and in Established state. In GitOps, bundling CRDs and CRs in the same unmanaged sync causes race conditions. I solve this using ArgoCD Sync Waves: wave 0 installs CRDs, wave 1 deploys the operator controllers, and wave 2 deploys the custom resources, ensuring zero deployment race conditions."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← GitOps & Continuous Delivery Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [GitOps & Continuous Delivery Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

