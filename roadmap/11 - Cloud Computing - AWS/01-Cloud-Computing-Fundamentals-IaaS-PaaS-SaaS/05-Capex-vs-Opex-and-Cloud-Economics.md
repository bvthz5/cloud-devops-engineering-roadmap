# 05 - CapEx vs. OpEx, TCO & Cloud Economics

> **Focus:** Financial modeling, Capital Expenditure (CapEx) vs. Operational Expenditure (OpEx), Total Cost of Ownership (TCO) equation, and the 3 FinOps phases (Inform, Optimize, Operate).

---

## 💰 1. CapEx vs. OpEx Breakdown

```text
CapEx (On-Premises): Heavy Initial Investment        OpEx (Cloud Computing): Pay-As-You-Go
         Cost ($)                                             Cost ($)
            ▲                                                    ▲
      $2M   │ ┌──────────────┐                             $40k  │     /\  /\  Peak
            │ │ Hardware Buy │                                   │    /  \/  \
            │ └──────────────┘                                   │   /        \ Off-peak
         $0 └─────────────────► Time                          $0 └─────────────────► Time
              Year 1   Year 2                                      Month 1   Month 2
```

| Financial Dimension | Capital Expenditure (CapEx) | Operational Expenditure (OpEx) |
| :--- | :--- | :--- |
| **Accounting Model** | Fixed asset purchases; depreciated over 3 to 5 years. | Operating business expense; deducted from taxes in the current year. |
| **Cash Flow Impact** | Massive upfront cash commitment before software is built. | Predictable monthly operational invoices matched to revenue. |
| **Capacity Risk** | Over-provisioning wastes capital; under-provisioning causes crashes. | Elastic; stretch resources during sales and shrink when idle. |

---

## 🧮 2. The Total Cost of Ownership (TCO) Equation

On-premises hardware is only the tip of the iceberg:

$$	ext{True On-Premises TCO} = 	ext{Server Hardware} + 	ext{Storage SANs} + 	ext{Network Switches} + 	ext{Datacenter Floor Rent} + 	ext{Power & Cooling} + 	ext{Hardware Warranties} + 	ext{24/7 Sysadmin Salaries} + 	ext{3-Yr Hardware Refresh}$$

---

## 🔄 3. The FinOps Lifecycle

1. **Inform (Visibility):** Tag 100% of resources (`Environment`, `Owner`, `Project`). Visualize daily spend using AWS Cost Explorer and AWS Budgets.
2. **Optimize (Rate & Usage Reduction):** Right-size oversized EC2 instances via AWS Compute Optimizer; convert `gp2` EBS disks to `gp3` (saves 20%); commit to Compute Savings Plans.
3. **Operate (Governance):** Automate shutdown of development instances outside business hours using AWS EventBridge and Systems Manager.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - AWS Shared Responsibility Model](./04-AWS-Shared-Responsibility-Model.md) | [Index](../../../README.md) | [06 - AWS Management Interfaces Console CLI SDKs IaC →](./06-AWS-Management-Interfaces-Console-CLI-SDKs-IaC.md) |
