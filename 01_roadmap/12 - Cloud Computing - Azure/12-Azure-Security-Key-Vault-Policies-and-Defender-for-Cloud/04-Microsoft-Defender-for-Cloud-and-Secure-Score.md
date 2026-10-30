# 04 - Microsoft Defender for Cloud: CSPM, CWPP & Secure Score Architecture

> **Focus:** Cloud Security Posture Management (CSPM) and Cloud Workload Protection (CWPP) in Microsoft Azure. Deep dive into Microsoft Defender for Cloud, Secure Score calculation, Defender for Servers/Containers/Storage, regulatory compliance standards, and multi-cloud protection across AWS and GCP.

---

## 🛡️ 1. What, Why, and History of Defender for Cloud

### What is Microsoft Defender for Cloud?
Microsoft Defender for Cloud is an enterprise cloud security solution that unifies:
1. **Cloud Security Posture Management (CSPM):** Continuously assesses resources against hardened security benchmarks, calculates your **Secure Score**, and provides prioritized remediation steps.
2. **Cloud Workload Protection Platform (CWPP):** Real-time, AI-driven threat detection and threat prevention across servers, Kubernetes containers, serverless functions, SQL databases, and storage accounts.

### Why do Modern Enterprises Require Continuous Security Monitoring?
1. **Misconfiguration is the #1 Cause of Cloud Breaches:** 99% of cloud security failures occur because an engineer left an S3 bucket or blob container public, forgot to enable disk encryption, or left port 22/3389 wide open to `0.0.0.0/0`.
2. **Dynamic Workload Drift:** Infrastructure deployed today may develop vulnerabilities tomorrow as new CVEs are discovered.
3. **Multi-Cloud Compliance:** Large enterprises operate across Azure, AWS, and GCP. Defender for Cloud connects natively to AWS and GCP accounts to provide a single unified pane of glass.

### 📜 History & Evolution:
- **2015:** Microsoft launches **Azure Security Center (ASC)** providing basic security recommendations.
- **2020:** Microsoft introduces the modern **Secure Score** percentage metric.
- **2021:** Rebranded to **Microsoft Defender for Cloud**, consolidating Azure Security Center and Azure Defender into a single unified multi-cloud product.

---

## ⚖️ 2. Architectural Comparison: CSPM vs. CWPP

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              DEFENDER FOR CLOUD ARCHITECTURE                           │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ PILLAR 1: CSPM (Cloud Security Posture Management) - FOUNDATIONAL / FREE               │
│   • Focus: PREVENTION & HARDENING.                                                     │
│   • Continuous vulnerability scanning against Microsoft Cloud Security Benchmark (MCSB)│
│   • Calculates your organizational SECURE SCORE (e.g., 78% compliant).                 │
│   • Detects drift: "Storage accounts without HTTPS", "VMs missing disk encryption".    │
│                                                                                        │
│ PILLAR 2: CWPP (Cloud Workload Protection Platform) - DEFENDER PLANS (PAID)            │
│   • Focus: ACTIVE THREAT DETECTION & INCIDENT RESPONSE.                                │
│   • Defender for Servers: Endpoint detection, vulnerability scanning (Qualys/MDVM),    │
│     File Integrity Monitoring (FIM), Just-In-Time (JIT) VM Access.                     │
│   • Defender for Containers: Runtime threat detection in AKS, ACR image vulnerability  │
│   • Defender for Storage: Detects malware uploads, anomalous mass downloads, exfiltration│
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 📈 3. How Secure Score is Calculated

Your **Secure Score** is a numeric percentage (0% to 100%) indicating how resilient your cloud estate is against known security threats:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              SECURE SCORE CALCULATION FORMULA                          │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ Secure Score = (Points Awarded for Fully Remediated Security Controls)                  │
│                ─────────────────────────────────────────────────────────  × 100        │
│                (Total Possible Points across All Applicable Controls)                  │
│                                                                                        │
│ • Grouped into Security Controls (e.g., "Enable MFA", "Restrict Unauthorized Network"). │
│ • All-or-Nothing Scoring per Control: You only receive the points for a control when   │
│   100% of affected resources within that control are remediated!                       │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Practical Implementation: Enabling Defender Plans via CLI

```bash
# 1. Enable Microsoft Defender for Virtual Machines (Defender for Servers)
az security pricing create \
  --name "VirtualMachines" \
  --tier "Standard" \
  --sub-plan "P2"

# 2. Enable Defender for Storage Accounts (Malware Scanning & Threat Detection)
az security pricing create \
  --name "StorageAccounts" \
  --tier "Standard"

# 3. Retrieve current Secure Score percentage across subscription
az security secure-scores list --output table
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Azure Policy Architecture Definitions Initiatives Assignments](./03-Azure-Policy-Architecture-Definitions-Initiatives-Assignments.md) | [Index](../../../README.md) | [05 - Azure Sentinel Cloud Native SIEM and SOAR →](./05-Azure-Sentinel-Cloud-Native-SIEM-and-SOAR.md) |
