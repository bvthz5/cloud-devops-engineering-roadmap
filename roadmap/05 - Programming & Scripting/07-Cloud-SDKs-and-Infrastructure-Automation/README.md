# 07 - Cloud SDKs and Infrastructure Automation

While declarative Infrastructure as Code (IaC) tools like Terraform and Pulumi excel at provisioning static resources, modern Platform Engineering and Site Reliability Engineering require **programmatic Cloud SDK automation**. Automated Janitor scripts, continuous cost optimization engines, dynamic auto-remediation loops, custom security auditing bots, and multi-tenant onboarding workflows are driven by Cloud SDKs in Python, Go, and PowerShell.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Cloud SDK Architecture: Client vs Resource Models](./01-Cloud-SDK-Architecture-Client-vs-Resource-Models.md) | Low-level wire clients vs high-level resource abstractions, thread safety, connection pooling. |
| 02 | [AWS Boto3 Deep Dive: Paginators, Waiters & Config](./02-AWS-Boto3-Deep-Dive-Paginators-Waiters-and-Config.md) | `client()` vs `resource()`, `botocore.config.Config`, token pagination, waiters, STS AssumeRole. |
| 03 | [Azure SDK for Python & Identity Libraries](./03-Azure-SDK-for-Python-and-Identity-Libraries.md) | `azure-identity` (`DefaultAzureCredential`), `ItemPaged`, `LROPoller` for async operations. |
| 04 | [Google Cloud Client Libraries & Service Accounts](./04-Google-Cloud-Client-Libraries-and-Service-Accounts.md) | Modern google-cloud-* libraries, ADC (`google.auth.default`), Protobuf object handling. |
| 05 | [Automated Cost Optimization & Cloud Janitor Scripts](./05-Automated-Cloud-Cost-Optimization-and-Janitor-Scripts.md) | Purging unattached EBS volumes, aged snapshots, idle Elastic IPs, orphaned disks with notifications. |
| 06 | [Security Scanning & Compliance Automation](./06-Security-Scanning-and-Compliance-Automation-with-SDKs.md) | Detecting public S3/GCS buckets, 0.0.0.0/0 Security Groups, non-MFA IAM users, auto-quarantine. |
| 07 | [Real-World Scenarios & Outage Post-Mortems](./07-Real-World-Scenarios.md) | API throttling storms (`RequestLimitExceeded`), STS expiration mid-batch, stale pagination drops. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Debugging wire-level HTTP traces (`botocore.logging`), IAM permission context debugging, Moto/LocalStack. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 senior Cloud/DevOps interview scenarios on SDK scalability, retries, and multi-cloud patterns. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Multi-region AWS EBS Janitor with Waiters; Lab 2: Azure Tag Enforcer; Lab 3: Multi-cloud secrets. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density cheat sheet for Boto3, Azure SDK, Google Cloud SDK, and production retry patterns. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - PowerShell Core](../06-PowerShell-Core-for-Cloud-and-DevOps/README.md) | [README](./README.md) | [01 - Cloud SDK Architecture](./01-Cloud-SDK-Architecture-Client-vs-Resource-Models.md) |
