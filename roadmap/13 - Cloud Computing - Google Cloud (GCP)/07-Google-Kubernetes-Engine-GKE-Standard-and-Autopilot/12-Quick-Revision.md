# Quick Revision Notes - Google Kubernetes Engine (GKE Standard & Autopilot)

> **Module**: Google Kubernetes Engine (GKE Standard & Autopilot)

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

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (08-Google-Cloud-Run-and-Cloud-Functions) →](../08-Google-Cloud-Run-and-Cloud-Functions/01-Cloud-Run-Architecture-Fully-Managed-Serverless-Containers.md) |
