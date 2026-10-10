# 06 - Auto Scaling Groups (ASG): Target Tracking, Step Scaling & Lifecycle Hooks

> **Focus:** Elastic compute scaling, Min / Desired / Max capacity parameters, Launch Templates, Target Tracking scaling policies, Cooldown timers, and instance termination policies.

---

## 📈 1. Auto Scaling Group (ASG) Architecture

An **Auto Scaling Group (ASG)** monitors your applications and automatically adjusts capacity to maintain steady, predictable performance at the lowest possible cost:

```text
                    Incoming Web Traffic Surges
                                 │
                                 ▼
           CloudWatch Metric Alarm: Average CPU > 60%
                                 │
                                 ▼
              Scale-Out Action Adds 2 EC2 Instances
    ┌─────────────────────────────────────────────────────────┐
    │                Auto Scaling Group (ASG)                 │
    │  [EC2-1]  [EC2-2]   ◄── [EC2-3]  [EC2-4] Added          │
    └─────────────────────────────────────────────────────────┘
                                 │
                 Cooldown Period (e.g. 300 Seconds)
               (Prevents rapid fluctuation / thrashing)
                                 │
                                 ▼
            Traffic Normalizes: Average CPU < 25%
                                 │
                                 ▼
              Scale-In Action Terminates 1 EC2 Instance
```

### ⚙️ The 3 Core Capacity Dimensions:
- **Minimum Size:** The lowest number of instances that will ever run (floor for high availability, e.g., `2`).
- **Maximum Size:** The ceiling preventing financial runaway costs (e.g., `10`).
- **Desired Capacity:** The current active instance count. ASG scales this number dynamically between Min and Max.

---

## 🎯 2. ASG Scaling Policy Types

1. **Target Tracking Scaling (Most Common):** You define a target metric (e.g., "Keep Average CPU utilization at 50%"). ASG automatically calculates how many instances to add or remove to track that target.
2. **Step Scaling:** Adds instances in graduated steps based on the size of the metric breach (e.g., If CPU $> 60%$, add 1 VM; if CPU $> 80%$, add 3 VMs).
3. **Scheduled Scaling:** Scales based on predictable calendar events (e.g., scale up to 10 instances every weekday at 8:00 AM).
4. **Predictive Scaling:** Uses machine learning models to forecast daily and weekly traffic trends and pre-warms instances ahead of time.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Elastic Load Balancers ALB NLB GWLB](./05-Elastic-Load-Balancers-ALB-NLB-GWLB.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
