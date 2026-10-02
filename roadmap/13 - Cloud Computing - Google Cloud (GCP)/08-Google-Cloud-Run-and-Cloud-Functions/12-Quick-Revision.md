# Quick Revision Notes - Google Cloud Run & Cloud Functions

> **Module**: Google Cloud Run & Cloud Functions

---

## ⚡ Key Cheat Sheet

| Service | Primary Purpose | Key Architectural Highlight |
|---|---|---|
| **GKE Autopilot** | Hands-off managed Kubernetes | Pay per Pod resource request |
| **Cloud Run** | Serverless container execution | Scales to zero, handles HTTP/gRPC |
| **Cloud Build** | Serverless CI/CD build service | Configured via `cloudbuild.yaml` |
| **Artifact Registry** | OCI container & package storage | Replaces legacy Container Registry (GCR) |
| **Cloud Operations** | Monitoring, Logging, Tracing | Integrated Ops Agent for system metrics |

---

## 📝 Top 5 Rules to Remember
1. **Use GKE Autopilot for simplified operations** unless custom kernel/node tuning is explicitly required.
2. **Use Artifact Registry over legacy GCR** (`gcr.io` is deprecated).
3. **Use Workload Identity for all GKE workloads** accessing GCP APIs.
4. **Export critical logs using Log Sinks** to BigQuery for SQL analytics or GCS for long-term compliance storage.
5. **Set `--min-instances` on Cloud Run** if cold-start latency sensitivity is required.
