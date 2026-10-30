# Troubleshooting Guide - Multi-Cloud Observability & Centralized Telemetry

> **Module**: Multi-Cloud Observability & Centralized Telemetry

---

## 🔍 Common Issue 1: OpenTelemetry Collector Memory Spikes & OOM Kills

### Symptom
OpenTelemetry Collector Pods running on GKE/EKS clusters experience high memory usage and get terminated by Kubernetes OOMKilled (`Exit Code 137`).

### Root Cause & Resolution
1. **High Log/Metric Volume**: Ingestion rates exceed collector memory limits during traffic surges.
2. **Batch Processor Tuning**: Configure `batch` processor with `send_batch_max_size` and `timeout` settings.
3. **Memory Limiter Processor**: Add `memory_limiter` processor in OTel configuration as the first pipeline item to drop or throttle data before memory thresholds are breached.

---

## 🔍 Common Issue 2: Cloud Custodian Policy Execution Failures

### Symptom
Cloud Custodian serverless policy fails to execute with `ClientError: AccessDenied` when attempting to release unattached EIPs or delete unattached EBS disks.

### Root Cause & Resolution
1. **Insufficient Execution IAM Role**: The AWS Lambda execution role assigned to Cloud Custodian lacks `ec2:ReleaseAddress` or `ec2:DeleteVolume` permissions.
2. **Least Privilege Attachment**: Attach the recommended Cloud Custodian IAM policy specifying exact resource modification permissions.
3. **Dry-Run Testing**: Run policies in `--dryrun` mode locally to inspect matching resource JSON payloads before enabling automated deletion actions.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
