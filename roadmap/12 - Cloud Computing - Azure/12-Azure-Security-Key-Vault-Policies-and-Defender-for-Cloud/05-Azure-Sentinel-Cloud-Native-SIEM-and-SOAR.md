# 05 - Microsoft Sentinel: Cloud-Native SIEM & SOAR Architecture

> **Focus:** Security Operations Center (SOC) engineering in Azure. Deep dive into Microsoft Sentinel architecture, Data Connectors, Analytic Rules, Fusion ML multi-stage attack correlation, Incident Investigation Graphs, and automated SOAR Playbooks (Azure Logic Apps).

---

## 🛡️ 1. What, Why, and History of Microsoft Sentinel

### What is Microsoft Sentinel?
Microsoft Sentinel is a scalable, cloud-native **Security Information and Event Management (SIEM)** and **Security Orchestration, Automated Response (SOAR)** solution delivered as a fully managed service built directly on top of Azure Log Analytics.

### Why did Cloud-Native SIEM Replace Legacy On-Prem SIEMs?
1. **Zero Infrastructure Maintenance:** Traditional on-premises SIEMs (like legacy ArcSight or on-prem Splunk clusters) require dedicated database administrators, complex storage area networks (SANs), and server patching. Sentinel scales automatically to ingest terabytes of logs per day with zero server management.
2. **Cloud-Speed AI Correlation (Fusion):** Correlates low-fidelity security signals across disparate systems (e.g., an abnormal login in Entra ID + an anomalous PowerShell execution in AWS + a mass blob download in Azure) into a single high-priority multi-stage incident.
3. **Automated Incident Remediation (SOAR):** Automatically responds to threats within seconds by isolating infected VMs, disabling compromised user accounts, or blocking malicious IPs on corporate firewalls.

### 📜 History & Evolution:
- **September 2019:** Launched into General Availability as **Azure Sentinel**.
- **2021:** Rebranded to **Microsoft Sentinel** to emphasize comprehensive multi-cloud and cross-platform capabilities (including native AWS CloudTrail and Google Cloud connectors).
- **2023–Present:** Deep integration with **Security Copilot**, allowing generative AI to draft incident summaries, reverse-engineer malware scripts, and write KQL queries from plain English prompts.

---

## 🔄 2. The 4 Operational Pillars of Microsoft Sentinel

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              THE 4 PILLARS OF MICROSOFT SENTINEL                       │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. COLLECT (Data Ingestion across Hybrid & Multi-Cloud):                               │
│    • 100+ native Data Connectors: Microsoft Entra ID, Microsoft 365, Azure Activity,   │
│      AWS CloudTrail, Google Cloud IAM, Cisco Firewalls, Linux Syslog, Windows CEF.     │
│                                                                                        │
│ 2. DETECT (Threat Analytics & AI Detection):                                           │
│    • Scheduled KQL Analytics Rules running against ingested Log Analytics tables.      │
│    • Machine-learning Fusion engine correlates anomalous events across kill-chains.    │
│                                                                                        │
│ 3. INVESTIGATE (Visual Triage & Root Cause Analysis):                                  │
│    • Generates actionable Incidents assigned to SOC analysts.                          │
│    • Interactive visual entity graph mapping IPs, users, hosts, and file hashes.       │
│                                                                                        │
│ 4. RESPOND (Automated SOAR Orchestration):                                             │
│    • Playbooks built on Azure Logic Apps execute automated containment workflows.       │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔍 3. Threat Hunting: Example KQL Analytics Detection Rule

Detecting **Brute-Force Password Spray Attacks followed by a Successful Login**:

```kusto
// Sentinel Analytics Rule: Detect Brute Force followed by Success
let timeframe = 1h;
let threshold = 5;
SigninLogs
| where TimeGenerated >= ago(timeframe)
| summarize
    FailedAttempts = countif(ResultType != 0),
    SuccessfulLogins = countif(ResultType == 0),
    IPAddresses = make_set(IPAddress),
    Applications = make_set(AppDisplayName)
    by UserPrincipalName
| where FailedAttempts >= threshold and SuccessfulLogins > 0
| project UserPrincipalName, FailedAttempts, SuccessfulLogins, IPAddresses, Applications
```

---

## ⚡ 4. SOAR Playbooks: Automated Threat Containment

When the KQL rule above fires an incident, Microsoft Sentinel triggers an automated **Playbook (Azure Logic App)**:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AUTOMATED SOAR PLAYBOOK FLOW                              │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ Step 1: Sentinel fires incident "Brute Force Attack Detected: user@contoso.com".       │
│ Step 2: Logic App Playbook triggers automatically.                                     │
│ Step 3: Action 1 ➔ Queries Microsoft Entra ID: Revokes all active refresh tokens.      │
│ Step 4: Action 2 ➔ Resets user password to random secure hash; forces MFA reset.       │
│ Step 5: Action 3 ➔ Posts detailed incident alert to SOC Team's private Slack/Teams.     │
│ Step 6: Action 4 ➔ Creates high-priority incident ticket in ServiceNow ITSM.           │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 5. Practical CLI: Enabling Microsoft Sentinel on a Workspace

```bash
# 1. Enable Microsoft Sentinel on an existing Log Analytics Workspace
az sentinel onboarding-state create \
  --name "default" \
  --resource-group "rg-security-soc" \
  --workspace-name "law-security-hub"

# 2. Connect the Azure Activity Data Connector
az sentinel data-connector create \
  --name "AzureActivityConnector" \
  --resource-group "rg-security-soc" \
  --workspace-name "law-security-hub" \
  --kind "AzureSecurityCenter" \
  --subscription-id $(az account show --query id -o tsv)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Microsoft Defender for Cloud and Secure Score](./04-Microsoft-Defender-for-Cloud-and-Secure-Score.md) | [Index](../../../README.md) | [06 - Azure Web Application Firewall WAF and DDoS Protection →](./06-Azure-Web-Application-Firewall-WAF-and-DDoS-Protection.md) |
