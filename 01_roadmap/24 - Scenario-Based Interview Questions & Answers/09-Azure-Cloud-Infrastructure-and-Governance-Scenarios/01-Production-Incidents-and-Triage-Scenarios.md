# Azure Cloud Infrastructure & Governance Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: Azure Kubernetes Service (AKS) Pod IP Exhaustion with Azure CNI

### 🚨 The Production Scenario
New deployments in an AKS cluster using Azure CNI fail to schedule with 'FailedCreatePodSandBox: NetworkPlugin cni failed to set up pod network: no IP addresses available in subnet'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Standard Azure CNI allocates a static pool of IP addresses from the host VNet subnet to every node upfront (e.g., 30 or 110 IPs per node) regardless of whether pods are actually running. As node count scaled, the /24 subnet ran completely out of allocatable IP space.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Check subnet available IPs in Azure Virtual Network via Azure CLI.
- Verify pod status and kubelet events in AKS using kubectl describe pod.
- Migrate from traditional Azure CNI to Azure CNI Overlay or Azure CNI with Dynamic Pod IP Allocation.
- With Azure CNI Overlay, pods get private IPs from a private CIDR block (e.g., 10.244.0.0/16) that does not consume VNet IPs.
- Temporarily reduce max-pods per node or add a secondary subnet pool if migration is delayed.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Count allocated IP addresses in AKS subnet
az network vnet subnet show -g rg-prod-network -n snet-aks --vnet-name vnet-prod --query 'ipConfigurations'

# Inspect CNI IP allocation failure events
kubectl get events -A --field-selector reason=FailedCreatePodSandBox

# Check AKS network plugin and allocation mode
az aks show -g rg-prod -n aks-prod --query 'networkProfile'

# Enable Azure CNI Overlay to eliminate VNet IP depletion
az aks update -g rg-prod -n aks-prod --network-plugin azure --network-plugin-mode overlay

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Traditional Azure CNI pre-allocates node-level IP blocks directly from the VNet, making subnet exhaustion a very common trap. I resolve this by adopting Azure CNI Overlay. With Overlay, nodes receive VNet IPs, but pods receive private overlay IPs from a distinct internal CIDR, supporting up to 250 pods per node without consuming enterprise VNet address space."

---

## 📌 Scenario 2: Azure Key Vault Access Denied (403 Forbidden) from VM via Managed Identity

### 🚨 The Production Scenario
An Azure Virtual Machine configured with a System-Assigned Managed Identity attempts to retrieve a database connection string from Azure Key Vault, returning 'Caller is not authorized to perform action on resource (Forbidden - 403)'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Azure Key Vault supports two authorization models: Vault Access Policies and Azure Role-Based Access Control (RBAC). The Key Vault was switched to Azure RBAC authorization, but the VM's Managed Identity was only granted an old Access Policy rather than an RBAC role like 'Key Vault Secrets User', or Key Vault Firewall blocked the VM private IP.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect Key Vault authorization model: check if 'enableRbacAuthorization' is true.
- Verify VM Managed Identity Principal ID and its role assignments on the Key Vault scope.
- Check Key Vault Network Rules: verify if public access is disabled and if Private Endpoint or VNet subnet is allowed.
- Verify DNS resolution on the VM: ensure 'privatelink.vaultcore.azure.net' resolves to the private endpoint IP.
- Assign 'Key Vault Secrets User' role to the Managed Identity and verify secret retrieval.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Retrieve VM System-Assigned Managed Identity Principal ID
az vm identity show -g rg-app -n vm-worker --query 'principalId' -o tsv

# Inspect Key Vault RBAC mode and network rules
az keyvault show -g rg-sec -n kv-prod-secrets --query '{enableRbac:properties.enableRbacAuthorization, networkRules:properties.networkAcls}'

# Assign Key Vault Secrets User RBAC role
az role assignment create --role 'Key Vault Secrets User' --assignee <PRINCIPAL_ID> --scope /subscriptions/.../resourceGroups/rg-sec/providers/Microsoft.KeyVault/vaults/kv-prod-secrets

# Test IMDS token acquisition inside VM
curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -H Metadata:true

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When an Azure Managed Identity receives 403 on Key Vault, I first determine whether the vault uses Access Policies or Azure RBAC. In RBAC mode, 'Key Vault Secrets User' must be assigned at the vault scope. Next, I check network ACLs: if Key Vault is private, I ensure the VM resolves the Private DNS zone 'privatelink.vaultcore.azure.net' to the private endpoint IP rather than the public VIP."

---

## 📌 Scenario 3: ExpressRoute Circuit Redundancy Drops - BGP Asymmetric Routing Outage

### 🚨 The Production Scenario
During scheduled maintenance on one of two ExpressRoute circuits in an active-active setup, on-premises applications lose connection to Azure VMs intermittently, dropping 50% of packets.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Asymmetric routing occurred because on-premises routers were advertising routes with equal BGP AS-path length, but stateful firewalls on-premises or in Azure were inspecting the forward flow on circuit 1 while return packets traversed circuit 2, causing stateful firewalls to drop out-of-state packets.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect BGP session status and route tables on both ExpressRoute circuits via Azure CLI and Network Watcher.
- Check AS-Path Prepending configurations on on-premises BGP edge routers.
- Verify Azure route table effective routes on VMs using Azure Network Watcher.
- Enforce BGP Local Preference on on-premises edge routers and ensure BFD (Bidirectional Forwarding Detection) is active for fast sub-second failover.
- Review firewall asymmetric routing bypass or configure active-passive pathing using AS-Path prepending.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check primary ExpressRoute line status and packet drops
az network express-route get-stats -g rg-network -n er-primary

# Inspect BGP neighbor status and advertised prefixes
az network express-route list-route-tables-summary -g rg-network -n er-primary --path default --peering-name AzurePrivatePeering

# Inspect VM NIC effective route table and next hops
az network nic show-effective-route-table -g rg-app -n nic-vm1 -o table

# Test end-to-end hop latency and packet route path
az network watcher test-connectivity -g rg-network --source-resource vm-vm1 --dest-address 192.168.1.50 --dest-port 443

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Asymmetric routing across dual ExpressRoute circuits breaks stateful firewalls that expect symmetrical request/response flow. I verify BGP routes across both peerings, implement BGP AS-Path Prepending or Local Preference to guarantee deterministic primary/secondary pathing, and enable Bidirectional Forwarding Detection (BFD) so failover takes milliseconds rather than waiting for 90-second BGP hold timers."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← AWS Cloud Infrastructure & Solutions Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../08-AWS-Cloud-Infrastructure-and-Solutions-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Azure Cloud Infrastructure & Governance Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

