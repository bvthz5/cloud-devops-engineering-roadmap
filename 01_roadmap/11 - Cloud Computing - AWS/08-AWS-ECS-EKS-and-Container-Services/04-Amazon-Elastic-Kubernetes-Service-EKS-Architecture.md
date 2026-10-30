# 04 - Amazon Elastic Kubernetes Service (EKS) Architecture

> **Focus:** Enterprise managed Kubernetes, High-availability control plane across 3 AZs, Managed Node Groups, Karpenter autoscaling, and kubectl authentication.

---

## ☸️ 1. Amazon EKS Architecture

**Amazon EKS** runs the upstream Kubernetes control plane across multiple AWS Availability Zones to ensure high availability:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   AWS MANAGED CONTROL PLANE (Single Tenant)            │
│  Highly available API servers & etcd distributed across 3 AZs          │
└───────────────────────────────────┬────────────────────────────────────┘
                                    │ AWS VPC CNI / Cross-Account ENI
                                    ▼
┌────────────────────────────────────────────────────────────────────────┐
│                        CUSTOMER DATA PLANE (VPC)                       │
│                                                                        │
│   ┌────────────────────────┐  ┌────────────────────────────────────┐   │
│   │  Managed Node Groups   │  │         Karpenter Autoscaler       │   │
│   │  EC2 instances running │  │  Just-in-time, right-sized compute │   │
│   │  kubelet & container runtime  provisioning without node groups │   │
│   └────────────────────────┘  └────────────────────────────────────┘   │
└────────────────────────────────────────────────────────────────────────┘
```

---

### 💻 Connecting to EKS via AWS CLI & kubectl

```bash
# Configure local kubeconfig for Amazon EKS
aws eks update-kubeconfig \
  --region us-east-1 \
  --name production-eks-cluster

# Verify Node Health
kubectl get nodes -o wide
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - AWS Fargate Serverless Container Compute](./03-AWS-Fargate-Serverless-Container-Compute.md) | [Index](../../../README.md) | [05 - EKS Networking VPC CNI and IAM Roles for Service Accounts IRSA →](./05-EKS-Networking-VPC-CNI-and-IAM-Roles-for-Service-Accounts-IRSA.md) |
