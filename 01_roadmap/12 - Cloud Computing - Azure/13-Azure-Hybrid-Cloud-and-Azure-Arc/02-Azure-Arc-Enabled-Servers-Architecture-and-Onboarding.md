# 02 - Azure Arc-Enabled Servers: Agent Architecture & At-Scale Onboarding

> **Focus:** Managing physical and virtual servers outside of Azure. Deep dive into the Azure Connected Machine Agent internal daemons (`himds`, `GC_Service`, `ExtensionService`), networking requirements (outbound 443 only), Private Link Scopes (APLS), and automated at-scale onboarding via Service Principals.

---

## 💻 1. What, Why, and History of Arc-Enabled Servers

### What are Azure Arc-Enabled Servers?
Azure Arc-enabled servers allows you to manage Windows and Linux physical servers and virtual machines hosted outside of Azure (in your corporate on-premises datacenter, VMware vSphere, Nutanix, AWS EC2, or Google Cloud Compute Engine) as if they were native Azure Virtual Machines.

### Why do Operations Teams Onboard Servers to Azure Arc?
1. **Single Management Control Plane:** Eliminates separate consoles; view and manage on-premises VMware machines alongside native Azure VMs in the Azure Portal.
2. **Unified Guest Governance with Azure Policy:** Enforce guest OS compliance (e.g., password complexity, disabling insecure SSH ciphers, verifying installed software) using Azure Policy Guest Configuration across 5,000 hybrid servers.
3. **Automated Patch Management (Azure Update Manager):** Centrally schedule, approve, and execute Windows Updates and Linux security patches across all on-premises and multi-cloud servers from the Azure cloud.
4. **Local Passwordless Identity (himds):** Provides on-premises servers with an Azure Managed Identity! Applications running on-prem can authenticate passwordlessly to Azure Key Vault and Storage accounts!

---

## 🏗️ 2. The Azure Connected Machine Agent Architecture

The Connected Machine Agent consists of three core background daemon services:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                        CONNECTED MACHINE AGENT DAEMON SERVICES                         │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. HIMDS (Hybrid Instance Metadata Service):                                           │
│    • Emulates the Azure Instance Metadata Service (IMDS) at http://169.254.169.254.    │
│    • Issues short-lived OAuth 2.0 Azure Entra ID tokens to local on-prem workloads!    │
│                                                                                        │
│ 2. GUEST CONFIGURATION SERVICE (GC_Service):                                           │
│    • Audits and applies in-guest operating system compliance policies.                 │
│    • Evaluates Desired State Configuration (DSC) scripts inside Windows and Linux.     │
│                                                                                        │
│ 3. EXTENSION SERVICE:                                                                  │
│    • Manages Azure VM extensions on external servers.                                  │
│    • Dynamically installs and auto-upgrades the Azure Monitor Agent (AMA),             │
│      Microsoft Defender for Endpoint, and Custom Script extensions.                    │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 🌐 3. Networking Requirements: Outbound Port 443 Only

A common security concern is whether Azure Arc requires opening incoming firewall ports into the corporate datacenter:

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                              ARC NETWORK SECURITY GUARANTEE                             │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│ • ZERO INBOUND PORTS OPENED: Azure Arc NEVER requires inbound firewall ports!           │
│ • OUTBOUND ONLY: The Connected Machine Agent initiates all traffic outbound over        │
│   standard HTTPS (port 443) using TLS 1.2 / 1.3.                                        │
│ • PROXY SUPPORT: Fully supports corporate forward HTTP/HTTPS proxy authentication.     │
│ • PRIVATE ACCESS: Azure Arc Private Link Scope (APLS) allows agent communication to stay│
│   entirely on ExpressRoute private circuits without traversing the public internet!     │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Practical Automation: At-Scale Linux Onboarding Script

To onboard hundreds of Linux servers without manual interaction, use a dedicated Microsoft Entra Service Principal assigned the lightweight **Azure Connected Machine Onboarding** role:

```bash
#!/bin/bash
# automated-arc-onboarding.sh

# 1. Download the official Azure Arc installation package
wget https://aka.ms/azcmagent -O ~/install_linux_azcmagent.sh
bash ~/install_linux_azcmagent.sh

# 2. Connect the server to Azure via Service Principal credentials
azcmagent connect \
  --service-principal-id "00000000-0000-0000-0000-000000000000" \
  --service-principal-secret "YourSecureClientSecretHere" \
  --tenant-id "11111111-1111-1111-1111-111111111111" \
  --subscription-id "22222222-2222-2222-2222-222222222222" \
  --resource-group "rg-hybrid-servers" \
  --location "eastus" \
  --tags "Datacenter=Frankfurt-OnPrem,Environment=Production,ManagedBy=AzureArc"

# 3. Verify agent connection status
azcmagent show
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Azure Hybrid Cloud Strategy and Overview](./01-Azure-Hybrid-Cloud-Strategy-and-Overview.md) | [Index](../../../README.md) | [03 - Azure Arc Enabled Kubernetes Clusters →](./03-Azure-Arc-Enabled-Kubernetes-Clusters.md) |
