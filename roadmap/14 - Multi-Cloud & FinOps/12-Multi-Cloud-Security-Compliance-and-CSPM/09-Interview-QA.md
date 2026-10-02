# Interview Q&A - Multi-Cloud Security, Compliance & CSPM

> **Module**: Multi-Cloud Security, Compliance & CSPM

---

### Q1: What is Cloud Custodian and how is it used in FinOps and Cloud Security governance?
**Answer**:
Cloud Custodian is an open-source, stateless policy-as-code engine for managing cloud infrastructure. It uses YAML policies to filter cloud resources (e.g., unattached disks, untagged VMs, exposed S3 buckets) and perform automated remediation actions (e.g., tagging, stopping, encrypting, or deleting), reducing waste and enforcing security compliance across AWS, Azure, and GCP.

---

### Q2: What are the main components of the OpenTelemetry (OTel) architecture?
**Answer**:
1. **API & SDKs**: Language-specific libraries instrumenting application code to generate traces, metrics, and logs.
2. **OpenTelemetry Collector**: Vendor-agnostic proxy agent consisting of **Receivers** (ingesting data), **Processors** (filtering/batching data), and **Exporters** (sending data to backends like Jaeger, Datadog, or Prometheus).
3. **OTLP Protocol**: OpenTelemetry Line Protocol defining standardized data serialization format over gRPC/HTTP.

---

### Q3: What is the advantage of using OpenTofu or Terragrunt in a multi-cloud enterprise?
**Answer**:
- **OpenTofu**: Open-source, MPL-licensed fork of Terraform ensuring open community governance without BSL licensing restrictions.
- **Terragrunt**: Thin wrapper for Terraform/OpenTofu that keeps code **DRY (Don't Repeat Yourself)** by centralizing backend state configurations, multi-provider credentials, and module invocations across complex multi-account / multi-cloud environments.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
