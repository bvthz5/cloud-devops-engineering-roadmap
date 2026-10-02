# Real-World Scenarios - Multi-Cloud Observability & Centralized Telemetry

> **Module**: Multi-Cloud Observability & Centralized Telemetry

---

## 🏢 Scenario 1: Automated Cloud Waste Reclamation Engine

### Background
An enterprise operating thousands of instances across AWS and Azure accrues \$40,000/month in orphaned EBS volumes, unattached Azure Managed Disks, and idle non-production VMs running during weekends.

### Solution Architecture
1. **Cloud Custodian Serverless Deployment**: Deploy Cloud Custodian policy rules running as AWS Lambda functions and Azure Functions.
2. **Automated Notification & Deletion**: Tag unattached volumes for 7-day deletion grace period, notifying resource creators via Slack before automated purge.
3. **Scheduled Off-Hours Shutdown**: Automatically stop non-production development VMs every Friday at 7 PM and restart Monday at 7 AM, saving 65% compute spend.

---

## 🏢 Scenario 2: Vendor-Neutral Multi-Cloud Observability Stack

### Background
A financial organization running applications across AWS EKS, Azure AKS, and GCP GKE wants to centralize metrics and tracing into Grafana Tempo and Thanos without deploying separate vendor-locked proprietary agents.

### Solution Architecture
1. **OpenTelemetry Collector DaemonSet**: Deploy OTel Collector on all Kubernetes clusters across AWS, Azure, and GCP.
2. **Unified Data Pipeline**: OTel Collectors standardize trace identifiers (W3C Trace Context) and push metrics to Thanos and traces to Grafana Tempo.
3. **Single-Pane Dashboard**: Engineering teams view end-to-end multi-cloud microservice latency traces on a unified Grafana dashboard.
