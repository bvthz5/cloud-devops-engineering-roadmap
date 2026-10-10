# 04 - Azure Pipelines: Microsoft-Hosted vs. Self-Hosted Agents & Scale Set Pools

> **Focus:** Pipeline execution runtimes in Azure DevOps. Deep dive into Microsoft-Hosted Agents, Self-Hosted Agents on Linux/Windows, VMSS Elastic Agent Pools, KEDA containerized agents, network communication via outbound HTTPS (443), and caching strategies.

---

## 💻 1. What, Why, and History of Azure DevOps Build Agents

### What is a Build Agent?
A Build Agent is a compute engine (virtual machine or container) running the open-source Azure Pipelines Agent listener daemon. When a pipeline triggers, the agent pulls source code, executes shell commands, runs test suites, builds containers, and publishes artifacts.

### Why do Organizations Need Both Microsoft-Hosted and Self-Hosted Agents?
1. **Zero-Maintenance vs. Total Control:** Microsoft-hosted agents require zero OS patching or Docker maintenance, but wipe disk storage after every single job.
2. **Private Network (VNet) Connectivity:** Microsoft-hosted agents run in Azure public subnets and cannot reach private corporate VNets, on-premises datacenters, or private AKS API servers. Self-hosted agents reside directly inside your private subnets.
3. **Build Caching & Giant Monorepos:** Building a 50 GB Android or C++ monorepo takes 45 minutes if downloading dependencies from scratch every build. Self-hosted agents preserve local disk caches (e.g., Docker layers, maven repo), slashing build times down to 3 minutes.

### 📜 History & Evolution:
- **2015:** Xplat (Cross-platform) agent written in Node.js introduced.
- **2017:** Modern v2/v3 agent written in high-performance .NET Core released.
- **2021:** **Azure Virtual Machine Scale Set (VMSS) Agent Pools** launched, providing automated elastic scaling of self-hosted VMs that boot up on demand and destroy themselves after running jobs.
- **2023–Present:** Containerized agents scaled dynamically via **KEDA (Kubernetes Event-driven Autoscaling)** on AKS and Azure Container Apps.

---

## ⚖️ 2. Architectural Comparison: Microsoft-Hosted vs. Self-Hosted

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              AGENT ARCHITECTURAL COMPARISON                            │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ MICROSOFT-HOSTED AGENT:                                                                │
│   • Fresh clean VM instantiated for every single job run; wiped immediately after.     │
│   • Zero maintenance, but NO local Docker cache retention across jobs.                 │
│   • Runs on public internet; cannot reach internal private databases without VPN.      │
│                                                                                        │
│ SELF-HOSTED AGENT (In Customer VNet / On-Premises):                                    │
│   • Persistent VM or elastic container running inside your private subnet (10.0.1.0/24)│
│   • Retains local disk caches (Docker layers, npm/pip caches) for blazing speed.       │
│   • Outbound HTTPS (443) long-polling only; ZERO inbound firewall ports opened!        │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

| Criteria | Microsoft-Hosted Agents | Self-Hosted Agents | Azure VMSS Elastic Agents |
| :--- | :--- | :--- | :--- |
| **Maintenance & Patching**| **100% Microsoft Managed** | Customer maintains OS & tools | OS image updated by customer; VMs auto-cycled |
| **VNet / Private Access** | ❌ No (Public internet only)| ✅ **Yes (Inside private VNet)**| ✅ **Yes (Inside private VNet)** |
| **Hardware & GPU Options**| Fixed standard sizes (2 vCPU/7GB)| Custom sizing, Any GPU/NVMe | Custom VM series (D, E, F, N-Series) |
| **Maximum Job Duration** | 360 minutes (6 hours) | **Unlimited** (Configurable) | Unlimited (Configurable) |
| **Disk State Across Jobs**| Wiped completely clean | Persistent (Can cause disk fill)| Configurable: Wiped or reused |
| **Cost Model** | Included free tier / Parallel jobs| Customer pays underlying VM cost| Customer pays VM running time only |

---

## 🔄 3. Secure Agent Communication Architecture

A common misconception is that Azure DevOps needs an open firewall port to reach a self-hosted agent. **In reality, the connection is 100% Outbound:**

```text
┌─────────────────────────────────────────────────────────────────────────────────────────┐
│                             AGENT SECURE POLLING ARCHITECTURE                           │
├─────────────────────────────────────────────────────────────────────────────────────────┤
│ Customer Private Subnet (10.0.1.0/24)             Azure DevOps Cloud                    │
│ ┌─────────────────────────────┐                   ┌───────────────────────────────────┐ │
│ │ Self-Hosted Agent VM        │                   │ dev.azure.com/contoso             │ │
│ │                             │                   │                                   │ │
│ │ ┌─────────────────────────┐ │  Outbound HTTPS   │ ┌───────────────────────────────┐ │ │
│ │ │ Pipelines Agent Daemon  │ ├──────────────────┼>│ Message Queue / Job Dispatcher  │ │ │
│ │ └─────────────────────────┘ │   (Port 443 Only) │ └───────────────────────────────┘ │ │
│ └─────────────────────────────┘                   └───────────────────────────────────┘ │
│                                                                                         │
│   • Agent opens an outbound HTTPS session using a Personal Access Token (PAT)           │
│     or Service Principal.                                                               │
│   • Polls the job queue using long-polling HTTP requests.                               │
│   • When a build triggers, Azure DevOps sends the pipeline definition down the open TCP │
│     stream. The agent executes commands locally and streams logs back up over port 443! │
└─────────────────────────────────────────────────────────────────────────────────────────┘
```

---

## 💻 4. Practical Setup: Registering a Linux Self-Hosted Agent

```bash
# 1. Create a dedicated working directory on your Ubuntu VM
mkdir -p ~/azagent && cd ~/azagent

# 2. Download the latest agent tarball from GitHub
curl -O -L https://vstsagentpackage.azureedge.net/agent/3.238.0/vsts-agent-linux-x64-3.238.0.tar.gz
tar zxvf vsts-agent-linux-x64-3.238.0.tar.gz

# 3. Configure the agent interactively or unattended
./config.sh --unattended \
  --url https://dev.azure.com/contoso-enterprise \
  --auth pat \
  --token "YourAzureDevOpsPATTokenWithAgentPoolsManageScope" \
  --pool "Private-VNet-Pool" \
  --agent "ubuntu-build-agent-01" \
  --work "_work" \
  --runAsService

# 4. Install and start as a systemd service
sudo ./svc.sh install
sudo ./svc.sh start
sudo ./svc.sh status
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Azure Pipelines YAML Pipeline Architecture and Syntax](./03-Azure-Pipelines-YAML-Pipeline-Architecture-and-Syntax.md) | [Index](../../../README.md) | [05 - Service Connections Variables and Environments →](./05-Service-Connections-Variables-and-Environments.md) |
