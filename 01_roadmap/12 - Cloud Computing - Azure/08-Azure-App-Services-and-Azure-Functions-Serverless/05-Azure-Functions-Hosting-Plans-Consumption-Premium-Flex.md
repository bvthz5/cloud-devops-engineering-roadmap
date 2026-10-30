# 05 - Azure Functions Hosting Plans: Consumption, Premium, Dedicated & Flex Consumption

> **Focus:** Serverless compute architecture on Azure. Deep dive into hosting plan trade-offs, Cold Start mechanics, VNet integration, maximum execution timeouts, scaling triggers, and selecting the optimal plan for latency-critical vs. cost-optimized microservices.

---

## ⚡ 1. What, Why, and History of Azure Functions Hosting Plans

### What is a Functions Hosting Plan?
An Azure Functions Hosting Plan dictates the underlying compute infrastructure, scaling behavior, memory allocation, network connectivity, and billing model used to execute your serverless code.

### Why do Multiple Hosting Plans Exist?
Serverless is not one-size-fits-all:
1. **Cost Optimization for Sporadic Tasks:** Nightly batch processing or occasional webhook endpoints should never pay for idle VM compute (requires pure consumption billing).
2. **Cold Start Elimination for Latency-Critical APIs:** Customer-facing mobile APIs cannot tolerate a 3–5 second cold-start delay when a container or runtime warms up from zero.
3. **Private Corporate Network Integration:** Backend serverless workers querying private Azure SQL or on-premises databases must live inside a VNet, which base consumption plans do not support.

### 📜 History & Evolution:
- **2016:** Azure Functions v1 launches with the **Consumption Plan** (pure event-driven, billing per millisecond and execution count).
- **2019:** **Functions Premium Plan (Elastic Premium)** introduced, offering pre-warmed instances (zero cold start) and Regional VNet Integration.
- **2024:** Microsoft unveils **Functions Flex Consumption**, providing fast instance startup, VNet integration, and high concurrency on a true pay-per-execution serverless pricing model.

---

## ⚖️ 2. Architectural Comparison of Azure Functions Hosting Plans

| Feature / Criteria | Consumption Plan (`Y1`) | Functions Premium (`EP1–EP3`) | Dedicated Plan (`App Service`) | Flex Consumption |
| :--- | :--- | :--- | :--- | :--- |
| **Billing Model** | Pay-per-execution ($0.000016 / GB-s)| Hourly per vCPU & RAM (Min 1 instance)| Flat monthly App Service fee | Pay-per-execution (Per-instance memory)|
| **Cold Starts** | Possible (2–6 seconds on cold hit)| **Zero (Pre-warmed instances)** | Zero (Always-on VMs) | Near-zero (Optimized fast provisioning)|
| **VNet Integration** | ❌ No | ✅ **Yes (Full Regional VNet)** | ✅ **Yes (Full Regional VNet)** | ✅ **Yes (Regional VNet)** |
| **Default Execution Timeout**| 5 minutes (Max 10 minutes) | 30 minutes (Unbounded configurable)| 30 minutes (Unbounded configurable)| Configurable up to 30 minutes |
| **Maximum Scaling Limit** | 200 instances | 100 instances | Manual / Autoscale rule bound | High-scale instance concurrency |
| **Best Use Cases** | Infrequent webhooks, batch processing | Latency-sensitive APIs, VNet backends | Existing underutilized App Service Plans | Modern high-throughput APIs & VNets |

---

## 🧊 3. Cold Start Internals & Mitigation Strategies

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                              COLD START ANATOMY (CONSUMPTION PLAN)                     │
├────────────────────────────────────────────────────────────────────────────────────────┤
│ 1. INCOMING REQUEST ARRIVES (No idle instances allocated)                              │
│ 2. SCALE CONTROLLER PROVISIONING (~1.5s):                                               │
│    • Allocates physical sandbox VM in datacenter.                                      │
│ 3. RUNTIME INITIALIZATION (~1.2s):                                                     │
│    • Boots language host process (Node.js, .NET Core CLR, Python runtime).             │
│ 4. APPLICATION ASSEMBLY LOADING (~1.5s):                                               │
│    • Downloads and unpacks zip payload from Azure Storage; resolves heavy npm/pip pkgs.│
│ 5. CODE EXECUTION (Actual function logic executes).                                    │
│    TOTAL LATENCY: 4–6 SECONDS ON COLD HIT!                                             │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### 🛠️ Production Cold Start Mitigations:
1. **Enable "Run From Package":** Set app setting `WEBSITE_RUN_FROM_PACKAGE = 1`. Runs function directly from zip file without extracting thousands of individual files into the virtual disk.
2. **Trim Unnecessary Dependencies:** Eliminate bloated dependencies; use lightweight HTTP clients instead of monolithic enterprise SDKs.
3. **Upgrade to Functions Premium:** Maintain 1 pre-warmed instance (`Always Ready Instances = 1`). Inbound traffic instantly hits active memory with zero spin-up time.

---

## 💻 4. Practical CLI: Deploying a Premium Function with VNet Integration

```bash
# 1. Create a Premium Elastic Functions Plan (EP1 SKU)
az functionapp plan create \
  --name plan-func-premium \
  --resource-group rg-serverless \
  --location eastus \
  --sku EP1 \
  --is-linux

# 2. Create the Function App with Pre-warmed instances
az functionapp create \
  --name func-api-prod \
  --resource-group rg-serverless \
  --storage-account mystoragefunc \
  --plan plan-func-premium \
  --runtime node \
  --runtime-version 20 \
  --functions-version 4

# 3. Configure 1 Always-Ready pre-warmed instance to eliminate cold starts
az functionapp plan update \
  --name plan-func-premium \
  --resource-group rg-serverless \
  --min-instances 1 \
  --max-instances 20

# 4. Attach to Private Subnet via VNet Integration
az functionapp vnet-integration add \
  --name func-api-prod \
  --resource-group rg-serverless \
  --vnet vnet-corp \
  --subnet snet-functions
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Azure Functions Architecture Triggers and Bindings](./04-Azure-Functions-Architecture-Triggers-and-Bindings.md) | [Index](../../../README.md) | [06 - Azure Event Grid and Azure Service Bus →](./06-Azure-Event-Grid-and-Azure-Service-Bus.md) |
