# 03 - AWS Fargate: Serverless Container Compute Engine

> **Focus:** Serverless container operations, AWS Fargate vs. EC2 Launch Type, resource allocation (vCPU/RAM), billing mechanics, and zero-host maintenance.

---

## 🚀 1. AWS Fargate vs. EC2 Launch Type

```text
┌──────────────────────────────────────┐       ┌──────────────────────────────────────┐
│        EC2 Launch Type (IaaS)        │       │        AWS Fargate (Serverless)      │
│                                      │       │                                      │
│  • You manage the underlying EC2 VMs │       │  • No EC2 servers to manage!         │
│  • You handle OS security patching   │       │  • AWS provisions microVM per task   │
│  • Must configure cluster scaling    │       │  • Auto-scales per task dynamically  │
│  • Pay for running EC2 VMs 24/7      │       │  • Pay strictly per vCPU/RAM second  │
└──────────────────────────────────────┘       └──────────────────────────────────────┘
```

### 📊 Master Comparison Matrix:

| Feature | EC2 Launch Type | AWS Fargate |
| :--- | :--- | :--- |
| **Server Management** | High (OS patching, Docker updates, AMI rotation). | **Zero (Completely Serverless)**. |
| **Scaling Unit** | Must scale both EC2 hosts AND container tasks. | Scales directly at the container task level. |
| **Pricing Model** | Billed for underlying EC2 instance types 24/7. | **Billed per vCPU and RAM used per second**. |
| **GPU Support** | ✅ Yes (Full access to GPU EC2 instances). | Limited / Work in progress. |
| **Best Enterprise Use** | Steady-state predictable high-volume clusters. | **Microservices, batch processing, fluctuating traffic**. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Elastic Container Service ECS Task Definitions and Services](./02-Elastic-Container-Service-ECS-Task-Definitions-and-Services.md) | [Index](../../../README.md) | [04 - Amazon Elastic Kubernetes Service EKS Architecture →](./04-Amazon-Elastic-Kubernetes-Service-EKS-Architecture.md) |
