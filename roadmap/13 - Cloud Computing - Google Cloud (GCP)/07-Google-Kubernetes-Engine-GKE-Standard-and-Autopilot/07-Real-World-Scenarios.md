# Real-World Scenarios - Google Kubernetes Engine (GKE Standard & Autopilot)

> **Module**: Google Kubernetes Engine (GKE Standard & Autopilot)

---

## 🏢 Scenario 1: Enterprise Microservices & Containerized CI/CD Pipeline

### Background
A tech company is running microservices on Google Kubernetes Engine (GKE) and Cloud Run. They need an automated CI/CD deployment pipeline using Cloud Build, Artifact Registry, and Cloud Deploy with zero manual production cluster credentials.

### Solution Architecture
1. **Source & Build**: Developers commit code to GitHub, triggering Cloud Build triggers via Webhooks.
2. **Artifact Security**: Build containers are pushed to Artifact Registry, scanned for vulnerabilities, and signed via Binary Authorization attestations.
3. **Deployment**: Cloud Deploy manages progressive delivery (canary -> production) using Workload Identity to interact securely with GKE clusters.

---

## 🏢 Scenario 2: Centralized Log Aggregation & Observability Dashboard

### Background
An enterprise operating across 50 GCP projects requires a centralized audit log repository for compliance, threat analysis, and real-time monitoring.

### Solution Architecture
1. **Log Router Sink**: Configure Organization-level Log Sinks in Cloud Logging to capture `cloudaudit.googleapis.com` logs across all child folders and projects.
2. **Storage & Analytics**: Route logs to a central BigQuery Dataset for SQL analysis and a Cloud Storage bucket for 7-year archive retention.
3. **Alerting**: Set up Cloud Monitoring metric-based alerts to notify Security Operations teams via PagerDuty upon suspicious IAM modifications.
