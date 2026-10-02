# Quick Revision Notes - Spot Instances, Savings Plans & Reserved Capacity

> **Module**: Spot Instances, Savings Plans & Reserved Capacity

---

## ⚡ Key Cheat Sheet

| Feature | Key Mechanism | Best Used For |
|---|---|---|
| **Savings Plans / CUDs** | 1-3 year hourly spend commitment | Baseline production workloads (50-70% savings) |
| **Spot / Preemptible VMs** | Spare compute capacity at up to 90% off | Fault-tolerant, stateless batch/worker nodes |
| **Workload Identity** | OIDC token exchange via STS | Passwordless cross-cloud authentication |
| **Transit Gateway / NCC** | Hub-and-spoke cloud routing | Centralized multi-cloud VPC interconnectivity |
| **Infracost PR Check** | Shift-Left IaC cost estimation | Blocking unapproved PR cost increases in CI/CD |

---

## 📝 Top 5 Rules to Remember
1. **Never use static AWS IAM Access Keys in GCP or CI/CD pipelines**; always use Workload Identity Federation.
2. **Combine Savings Plans (for baseline) and Spot VMs (for scale-out)** to achieve maximum compute cost efficiency.
3. **Handle 2-minute Spot termination notices gracefully** using termination handlers and checkpointing.
4. **Use private interconnects (DirectConnect / ExpressRoute / Interconnect)** to avoid high public egress rates.
5. **Enforce Infracost guardrails in CI/CD** to gain cost visibility before infrastructure is provisioned.
