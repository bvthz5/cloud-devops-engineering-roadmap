# Azure Cloud Infrastructure & Governance Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: Azure DevOps Self-Hosted Agent Scales Out But Fails Job Execution Due to Missing VNet Peering

### 🚨 The Production Scenario
Ephemeral self-hosted Azure DevOps agent pools deployed via Azure Virtual Machine Scale Sets (VMSS) spin up successfully, but CI/CD pipeline deployment jobs immediately fail with 'Connection refused' when deploying to private AKS and Azure SQL instances.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The VMSS agent pool was deployed into an isolated subnet whose VNet lacked VNet Peering to the shared services and production hub VNets, and Network Security Groups (NSGs) blocked outbound port 443 and 1433.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Review Azure DevOps pipeline runner logs for the exact destination IP and port timeout.
- Check VMSS VNet route tables and VNet peering status with hub and spoke VNets.
- Verify Network Security Group (NSG) outbound rules on the agent subnet.
- Test name resolution of private endpoints (privatelink.database.windows.net) from the agent VNet.
- Link Azure Private DNS Zones to the agent VNet to enable DNS resolution of private endpoints.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Verify VNet peering status with target resource VNets
az network vnet peering list -g rg-cicd --vnet-name vnet-agents -o table

# Inspect agent subnet NSG egress rules
az network nsg rule list -g rg-cicd --nsg-name nsg-agents --query '[*].{Name:name,Access:access,Direction:direction,DestPort:destinationPortRange}' -o table

# Check if Private DNS Zone is linked to Agent VNet
az network private-dns link vnet list -g rg-dns -z privatelink.database.windows.net -o table

# Link Private DNS zone to Agent VNet
az network private-dns link vnet create -g rg-dns -n link-agents -z privatelink.database.windows.net -v vnet-agents -e false

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Self-hosted build agents running in private VNets require two things to communicate with private enterprise resources: VNet Peering to the spoke VNets (or Transit Hub) and Virtual Network links on Azure Private DNS Zones. Without the Private DNS link, agents resolve private PaaS endpoints to public IPs that fail firewall checks."

---

## 📌 Scenario 5: Azure Cosmos DB Request Unit (RU/s) Throttling (HTTP 429) Spikes

### 🚨 The Production Scenario
During billing cycle processing, an application reading and writing to Azure Cosmos DB experiences severe throughput drops, and client application logs fill with 'CosmosException: Response status code does not indicate success: 429 (RequestRateTooLarge)'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The Cosmos DB container partition key was poorly designed (e.g., using 'tenant_id' where 80% of records belong to one enterprise customer). This created a 'hot partition', exhausting the RU/s allocated to that physical partition while other partitions sat idle.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Query Azure Monitor metrics for Cosmos DB: 'Total Request Units' split by PartitionKeyRangeId and 'Normalized RU Consumption'.
- Identify hot partition key values generating disproportionate 429 throttling errors.
- Enable Cosmos DB Autoscale RU/s to dynamically adapt to traffic spikes.
- Redesign the partition key using a synthetic composite key (e.g., tenant_id + date or hash suffix) to evenly distribute writes across physical partitions.
- Configure client SDK retry policies with exponential backoff and jitter.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check container provisioned RU/s or autoscale limits
az cosmosdb sql container throughput show -g rg-data -a cosmos-prod -d appdb -n transactions

# Query 429 throttle rate over time
az monitor metrics list --resource <COSMOS_RESOURCE_ID> --metric 'TotalRequests' --filter 'StatusCode eq 429' --interval PT1M

# Upgrade container to Autoscale throughput
az cosmosdb sql container throughput update -g rg-data -a cosmos-prod -d appdb -n transactions --max-throughput 20000

# Verify container partition key paths
az cosmosdb sql container list -g rg-data -a cosmos-prod -d appdb --query '[*].{Name:name, PartitionKey:partitionKey.paths[0]}'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Cosmos DB 429 errors almost always stem from hot partitions rather than overall insufficient RU/s. When throughput is provisioned, it is split equally across physical partitions. If one partition key receives the majority of traffic, that physical partition throttles. I identify the hot partition via Azure Monitor metrics, enable Autoscale RU/s for burst headroom, and redesign the partition key using synthetic keys to spread data across storage partitions."

---

## 📌 Scenario 6: Azure Policy Blocks Deployment of Terraform Infrastructure via CI/CD

### 🚨 The Production Scenario
A Terraform pipeline attempting to provision a storage account in Azure Commercial region fails with: 'RequestDisallowedByPolicy: Resource was disallowed by policy. Policy: Require secure transfer and TLS 1.2 on storage accounts'.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Enterprise Azure Policy assigned at the Management Group scope enforces governance compliance by evaluating incoming ARM/Bicep/Terraform payloads. The Terraform code omitted explicit arguments for 'enable_https_traffic_only = true' and 'min_tls_version = "TLS1_2"', violating the policy rule.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Inspect the pipeline error JSON to extract the exact PolicyAssignmentId and PolicyDefinitionId.
- Run 'az policy assignment show' to read the policy rule definition and parameters.
- Update the Terraform module to explicitly define all required compliance parameters.
- Run 'terraform plan' and validate against policy compliance using Azure Policy compliance scans.
- Re-run the CI/CD pipeline to verify successful provisioning.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect Azure Policy definition details
az policy assignment show --name 'require-storage-tls12' --scope '/subscriptions/12345678-abcd-1234-abcd-1234567890ab'

# List non-compliant resources in resource group
az policy state list --resource-group rg-prod --filter 'complianceState eq NonCompliant'

# Trigger on-demand policy compliance re-evaluation
az policy state trigger-scan -g rg-prod

# Validate Terraform syntax before reapplying
terraform validate

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Azure Policies enforce guardrails at the ARM control plane layer before resources exist. When a policy blocks Terraform, I extract the policy definition ID from the error output to view the required compliance properties. Then I codify those parameters into the Terraform module—such as minimum TLS version and private endpoint enforcement—and integrate policy-as-code linting like Trivy or Checkov into CI to catch violations before deployment."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Azure Cloud Infrastructure & Governance Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Azure Cloud Infrastructure & Governance Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

