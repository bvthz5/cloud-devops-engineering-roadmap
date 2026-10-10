# 02 - EC2 Pricing Models: On-Demand, Spot Instances, Reserved Instances & Savings Plans

> **Focus:** Compute purchasing economics, On-Demand baseline, Spot Instance interruption handling (2-minute warning), Reserved Instances (Standard vs. Convertible), and Savings Plans (Compute vs. EC2 Instance).

---

## 💰 1. The 4 EC2 Pricing Models

```text
Commitment: ZERO ◄───────────────────────────────────────────────────────► 1 to 3 YEARS
Discount:   0%   ◄───────────────────────────────────────────────────────► Up to 90%

┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│    ON-DEMAND    │    │      SPOT       │    │  SAVINGS PLANS  │    │ RESERVED INST.  │
│                 │    │                 │    │                 │    │                 │
│ Pay per second. │    │ Up to 90% off.  │    │ Up to 72% off.  │    │ Up to 72% off.  │
│ No commitment.  │    │ AWS can reclaim │    │ Hourly $/hr     │    │ 1 or 3 yr term. │
│ Highest price.  │    │ with 2-min warn │    │ commitment.     │    │ Standard / Conv.│
└─────────────────┘    └─────────────────┘    └─────────────────┘    └─────────────────┘
```

### 📊 Comprehensive Economic Matrix:

| Pricing Model | Financial Discount | Commitment Term | Interruption Risk | Best Enterprise Workload |
| :--- | :---: | :---: | :---: | :--- |
| **On-Demand** | 0% (Full Price) | None (Pay per second) | ❌ Zero | Unpredictable short-term workloads, dev/test spikes, new app benchmarking. |
| **Spot Instances** | **Up to 90%** | None | ⚠️ **Yes (2-minute warning)** | Batch processing, stateless container workers, CI/CD runners, machine learning training. |
| **Compute Savings Plans** | **Up to 66%** | 1 or 3 Years (`$/hour`) | ❌ Zero | Most flexible! Automatically applies across EC2, AWS Fargate, and AWS Lambda regardless of region or OS. |
| **EC2 Instance Savings Plans**| **Up to 72%** | 1 or 3 Years (`$/hour`) | ❌ Zero | High discount for steady-state instances within a specific region and instance family. |
| **Standard Reserved Instances**| **Up to 72%** | 1 or 3 Years | ❌ Zero | Steady-state databases and legacy servers with guaranteed instance type requirements. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - EC2 Instance Types Families and Generations](./01-EC2-Instance-Types-Families-and-Generations.md) | [Index](../../../README.md) | [03 - EBS Volume Types Snapshots and Instance Store →](./03-EBS-Volume-Types-Snapshots-and-Instance-Store.md) |
