# Real-World Scenarios - Google Cloud IAM & Service Accounts

> **Module**: Google Cloud IAM & Service Accounts

---

## 🏢 Scenario 1: Enterprise Landing Zone & Multi-Tenant Resource Governance

### Background
A enterprise financial institution is migrating 200+ microservices to GCP. They require strict network isolation between Production, Staging, and Development environments while maintaining centralized audit logging and identity management.

### Solution Architecture
1. **Folder Structure**: Create distinct Folders under the Root Organization for `Production`, `Staging`, `Development`, and `Shared-Services`.
2. **Shared VPC Network**: Implement a Shared VPC in a central Networking Project, delegating subnets to application service projects to isolate data planes while centralizing network security controls.
3. **Org Policies**: Enforce Org Policies prohibiting `externalIpAccess` across all non-public subnets and disabling service account key creation.

---

## 🏢 Scenario 2: Zero-Downtime DR & Regional Failover Architecture

### Background
A global e-commerce application running on GCP needs multi-region failover capability to handle regional outages without data loss.

### Solution Architecture
1. Deploy active-active compute workloads in `us-central1` and `us-east4` using Global HTTP(S) Load Balancing.
2. Utilize Multi-Region Cloud Storage buckets and Cloud Spanner / Cloud SQL with cross-region read replicas for continuous data replication.
3. Automate health checks and DNS failover policies to route user traffic seamlessly in under 30 seconds.
