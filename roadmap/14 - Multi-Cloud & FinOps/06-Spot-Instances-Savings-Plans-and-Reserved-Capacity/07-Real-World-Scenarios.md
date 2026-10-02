# Real-World Scenarios - Spot Instances, Savings Plans & Reserved Capacity

> **Module**: Spot Instances, Savings Plans & Reserved Capacity

---

## 🏢 Scenario 1: Multi-Cloud Rate Optimization & Spot Fleet Architecture

### Background
A high-performance analytics platform runs daily batch jobs processing terabytes of data across AWS EC2 and GCP Compute Engine. Infrastructure costs are unsustainable on on-demand pricing.

### Solution Architecture
1. **Spot / Preemptible Fleet**: Transition stateless processing worker nodes to Spot Instances (AWS Spot Fleet & GCP Spot VMs), cutting compute unit cost by 75%.
2. **Auto-Fallback to On-Demand**: Configure Auto Scaling groups with mixed instance types (e.g., `m5.large`, `m5a.large`, `c5.large`) to guarantee capacity availability during Spot terminations.
3. **Commitment Coverage**: Secure 3-year Compute Savings Plans for baseline control plane and database instances to lock in 50% discount rates.

---

## 🏢 Scenario 2: Zero-Trust Cross-Cloud Workload Authentication

### Background
A multi-cloud application running in GCP Cloud Run needs to read data from an Amazon S3 bucket without storing AWS IAM Access Keys in Secret Manager.

### Solution Architecture
1. **OIDC Federation**: Configure AWS IAM Workload Identity Federation with GCP as an OpenID Connect (OIDC) identity provider.
2. **Short-Lived STS Token**: GCP Cloud Run requests a short-lived OIDC token from GCP Metadata server and exchanges it for a 1-hour AWS STS temporary credential.
3. **Security Benefits**: Zero static credentials stored in code or secrets managers, eliminating key exposure risks.
