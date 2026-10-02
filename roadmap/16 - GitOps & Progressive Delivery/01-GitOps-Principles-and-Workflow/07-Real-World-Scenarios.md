# Real-World Scenarios - GitOps Principles & Workflow

> **Module**: GitOps Principles & Workflow

---

## 🏢 Scenario 1: Zero-Touch Disaster Recovery & Multi-Cluster Rebuilding

### Background
An enterprise financial institution loses an entire Kubernetes production cluster in `us-east-1` due to cloud provider infrastructure failure. They need to rebuild the cluster from scratch in `us-west-2` with zero manual manifest applications.

### Solution Architecture
1. **Cluster Bootstrapping**: Spin up an empty EKS cluster in `us-west-2` via Terraform.
2. **GitOps Agent Initialization**: Install ArgoCD / Flux and point the root Application / Kustomization to the Git `cluster-fleet` repository.
3. **Automated Rehydration**: ArgoCD automatically restores all 150 microservices, ingress controllers, RBAC policies, and ConfigMaps from Git in under 10 minutes.

---

## 🏢 Scenario 2: Automated Metric-Driven Canary Release with Argo Rollouts

### Background
An e-commerce website requires zero-downtime updates for its core Checkout service. Any deployment bug causing latency > 500ms or HTTP 5xx errors > 0.5% must instantly trigger a rollback before affecting all users.

### Solution Architecture
1. **Argo Rollout CRD**: Replace standard Kubernetes Deployment with an Argo Rollout defining a 5-step canary progression (`10%` -> `25%` -> `50%` -> `100%`).
2. **Prometheus AnalysisTemplate**: Configure real-time metric queries evaluating error rate and P99 latency during 5-minute pause windows.
3. **Automated Abort & Rollback**: If Prometheus returns HTTP 5xx rate > 0.5%, Argo Rollouts instantly aborts the rollout, restores 100% traffic to the stable version, and sends a PagerDuty alert.
