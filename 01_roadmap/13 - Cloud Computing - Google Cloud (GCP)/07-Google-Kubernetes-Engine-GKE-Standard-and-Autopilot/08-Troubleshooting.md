# Troubleshooting Guide - Google Kubernetes Engine (GKE Standard & Autopilot)

> **Module**: Google Kubernetes Engine (GKE Standard & Autopilot)

---

## 🔍 Common Issue 1: GKE Workload Identity Authentication Failures

### Symptom
Pods running in GKE fail to authenticate to GCP services (e.g., Cloud SQL, GCS) with `401 Unauthorized` or `403 Insufficient Permission`.

### Root Cause & Resolution
1. **Missing ServiceAccount Annotation**: Verify the Kubernetes ServiceAccount is annotated with the GCP Service Account email:
   ```bash
   kubectl annotate serviceaccount K8S_SA_NAME \
       --namespace K8S_NAMESPACE \
       iam.gke.io/gcp-service-account=GCP_SA_NAME@PROJECT_ID.iam.gserviceaccount.com
   ```
2. **Missing IAM Binding**: Ensure the IAM policy binding `roles/iam.workloadIdentityUser` is configured between the GCP Service Account and Kubernetes Service Account.
3. **Pod Spec**: Ensure `serviceAccountName: K8S_SA_NAME` is defined under the Pod spec.

---

## 🔍 Common Issue 2: Cloud Run Deployment Latency & Cold Starts

### Symptom
Cloud Run services experience periodic 2-5 second latency spikes on incoming requests after periods of inactivity.

### Root Cause & Resolution
1. **Cold Starts**: Container instances scale down to 0 when idle.
2. **Minimum Instances**: Configure minimum instances (`--min-instances=1` or higher) to keep warm container instances ready to serve traffic immediately.
3. **Image Optimization**: Reduce Docker image size and optimize runtime startup times (e.g., lightweight base images, lazy loading dependencies).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
