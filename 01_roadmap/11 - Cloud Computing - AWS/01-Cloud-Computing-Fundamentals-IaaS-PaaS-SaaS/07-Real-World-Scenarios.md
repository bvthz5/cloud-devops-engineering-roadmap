# 07 - Real-World Scenarios: Enterprise Datacenter Exit & FinOps Run

> **Focus:** Real on-the-job enterprise scenarios: Executing a 250-server on-premises datacenter migration to AWS and running an aggressive FinOps cost reduction initiative.

---

## 🏢 Scenario 1: Enterprise Datacenter Exit (CapEx ➔ OpEx Migration)

### 📌 Context & Problem:
A financial services firm runs 250 VMware ESXi virtual machines in a colocation facility in Chicago. Their server hardware warranty expires in 4 months, requiring a **$1.8M CapEx renewal** for new SAN storage and Dell rack servers.

### 🛠️ Solution Architecture:
1. **Discovery & Assessment:** Used AWS Application Discovery Service to inventory CPU, RAM, and disk IOPS across all 250 servers. Found that 40% of servers averaged $< 10%$ CPU utilization.
2. **Migration Execution (Rehost / Replatform):**
   - Deployed **AWS Application Migration Service (MGN)** to continuously replicate block storage over a 1 Gbps AWS Direct Connect link.
   - Migrated 45 self-managed SQL Server instances into **Amazon RDS Multi-AZ**, eliminating manual backup scripts and OS patching.
3. **Financial Outcome:**
   - Avoided the $1.8M upfront CapEx hardware check.
   - Total AWS monthly operating cost: **$42,500/month**.
   - Net 3-year TCO savings: **38.4% ($960,000 saved)**.

---

## 📉 Scenario 2: Aggressive FinOps Cloud Cost Reduction Run

### 📌 Context & Problem:
An e-commerce company noticed their monthly AWS invoice climbed from $35k to $118k over 12 months with no formal governance.

### 🛠️ Remediations Executed:
1. **Storage Conversion:** Ran an automation script converting 450 `gp2` EBS volumes to `gp3` (Baseline 3,000 IOPS, 20% cost reduction). **Saved $3,400/mo**.
2. **Idle Resource Clean-Up:** Deleted 68 unattached EBS volumes left over from terminated EC2 instances and 12 unused Elastic IPs. **Saved $1,200/mo**.
3. **Graviton CPU Architecture Migration:** Converted Node.js microservices from `m5.xlarge` (Intel) to `m6g.xlarge` (AWS Graviton ARM). Achieved 25% better throughput at 20% lower cost. **Saved $8,900/mo**.
4. **Compute Savings Plans:** Committed to a 1-year No-Upfront Compute Savings Plan for steady baseline usage. **Saved $22,000/mo**.
5. **Final Result:** Slashed bill from **$118,000/mo to $56,500/mo (52% reduction)** in 60 days.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - AWS Management Interfaces Console CLI SDKs IaC](./06-AWS-Management-Interfaces-Console-CLI-SDKs-IaC.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
