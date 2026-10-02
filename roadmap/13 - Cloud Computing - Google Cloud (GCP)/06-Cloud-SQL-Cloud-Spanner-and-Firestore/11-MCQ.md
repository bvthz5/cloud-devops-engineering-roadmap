# MCQ - Cloud SQL, Cloud Spanner & Firestore

> **Module**: Cloud SQL, Cloud Spanner & Firestore

---

### Question 1
Which GCP serverless compute engine allows deploying arbitrary container images listening on HTTP requests without managing underlying VM nodes?
- [ ] A) Compute Engine
- [x] B) Cloud Run
- [ ] C) App Service
- [ ] D) Google Cloud Functions Gen1

*Explanation: Cloud Run is a fully managed serverless platform that runs containerized applications directly.*

---

### Question 2
What component in Cloud Operations allows exporting logs from Cloud Logging to BigQuery or Cloud Storage?
- [ ] A) Log Metrics
- [x] B) Log Router Sink
- [ ] C) Cloud Audit Agent
- [ ] D) Ops Agent Alert

*Explanation: Log Router Sinks filter and direct incoming logs to destinations like BigQuery, GCS, or Pub/Sub.*

---

### Question 3
How does GKE Workload Identity authenticate Pods to GCP APIs without static service account keys?
- [ ] A) By assigning public IP addresses to Pods
- [x] B) By mapping a Kubernetes ServiceAccount to a GCP Service Account
- [ ] C) By mounting static SSH keys into Pod containers
- [ ] D) By disabling IAM checks on the GKE cluster

*Explanation: Workload Identity links Kubernetes ServiceAccounts directly with GCP Service Accounts for seamless OAuth 2.0 token generation.*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
