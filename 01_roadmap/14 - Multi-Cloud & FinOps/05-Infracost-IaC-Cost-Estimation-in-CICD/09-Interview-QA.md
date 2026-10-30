# Interview Q&A - Infracost: Shift-Left IaC Cost Estimation in CI/CD

> **Module**: Infracost: Shift-Left IaC Cost Estimation in CI/CD

---

### Q1: What is the difference between AWS Savings Plans and AWS Reserved Instances (RIs)?
**Answer**:
- **Savings Plans**: Offer flexible discounts (up to 72%) in exchange for a commitment to a consistent hourly spend (e.g., \$10/hour) over 1 or 3 years. Compute Savings Plans apply automatically across instance families, OS, regions, and compute types (EC2, Fargate, Lambda).
- **Reserved Instances**: Tied to specific instance types, OS, and regions (Standard RIs). Less flexible than Savings Plans.

---

### Q2: How does Workload Identity Federation work between multi-cloud providers?
**Answer**:
Workload Identity Federation leverages standard **OpenID Connect (OIDC)** or SAML 2.0. Workloads in one cloud (e.g., GCP or GitHub Actions) generate a signed OIDC JWT token. This token is sent to another cloud's Security Token Service (STS) (e.g., AWS STS or Azure Entra ID), which verifies the signature and issues temporary IAM credentials without needing static keys.

---

### Q3: How do private carrier hubs like Equinix Fabric reduce multi-cloud egress costs?
**Answer**:
Traversing public internet endpoints between AWS, Azure, and GCP incurs full internet egress pricing (\$0.08–\$0.12 per GB). Connecting cloud providers via private cloud interconnects (AWS DirectConnect, Azure ExpressRoute, GCP Interconnect) via a carrier hub like Equinix or Megaport reduces egress rates to \$0.02 per GB while lowering latency and improving throughput.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
