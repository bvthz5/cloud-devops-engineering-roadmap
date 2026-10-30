# 04 - Azure Functions (Serverless Compute) & Azure Container Instances (ACI)

> **Focus:** Event-driven serverless computing, Triggers vs. Bindings, Hosting plans (Consumption vs. Premium vs. Dedicated), Cold starts, Python code structure, and Azure Container Instances (ACI) batch jobs.

---

## ⚡ 1. Azure Functions Architecture: Triggers & Bindings

**Azure Functions** allows developers to execute event-driven code snippets without managing virtual machines. You pay only for the exact seconds your code runs.

```text
TRIGGER (What starts the function)
   ├── HTTP Trigger      (REST API requests)
   ├── Timer Trigger     (Cron schedule e.g., 0 0 * * *)
   ├── Blob Trigger      (New CSV uploaded to Storage)
   └── Queue Trigger     (Message lands in Azure Storage Queue)
             │
             ▼
      FUNCTION CODE (Runs in sandbox container)
             │
             ▼
BINDINGS (Declarative I/O without SDK boilerplate)
   ├── Input Binding     (Reads metadata from Cosmos DB)
   └── Output Binding    (Writes processed record to Storage)
```

---

## 💳 2. Hosting Plans: Consumption vs. Premium vs. Dedicated

| Plan | Billing Model | Scaling Speed | Scale to Zero? | Cold Starts? | Best Use Case |
| :--- | :--- | :--- | :---: | :---: | :--- |
| **Consumption** | Pay-per-execution (Milliseconds). First 1M executions free. | Dynamic automatic scale up to 200 instances. | ✅ **Yes** | ⚠️ **Yes (1–5s)** | Event-driven batch jobs, asynchronous data pipelines. |
| **Premium** | Per-second allocation of pre-warmed CPU/RAM. | Instant auto-scaling without cold starts. | ❌ No | ❌ **No Cold Starts** | Latency-critical APIs, VNet integrated private functions. |
| **Dedicated** | Flat monthly App Service Plan rate. | Manual or metric-based autoscale. | ❌ No | ❌ No | Steady workloads sharing compute with web apps. |

---

## 🐍 3. Python HTTP Trigger Function Example

```python
# function_app.py (Azure Functions Python v2 Model)
import azure.functions as func
import logging

app = func.FunctionApp()

@app.route(route="hello", methods=["GET"])
def hello_function(req: func.HttpRequest) -> func.HttpResponse:
    name = req.params.get("name", "World")
    logging.info(f"Triggered for name: {name}")
    return func.HttpResponse(f"Hello, {name}! Azure Functions Serverless is active.", status_code=200)
```

---

## 🐳 4. Azure Container Instances (ACI)

**Azure Container Instances (ACI)** is the fastest way to run Docker containers in the cloud without provisioning virtual machines or configuring Kubernetes clusters.

### 📊 ACI vs. Web Apps vs. AKS:

| Feature | Azure Container Instances (ACI) | App Service (Container) | Azure Kubernetes Service (AKS) |
| :--- | :--- | :--- | :--- |
| **Setup Complexity**| Very Low (Instant) | Low | High |
| **Orchestration** | ❌ No (Single containers/groups) | Basic | ✅ **Full Kubernetes** |
| **Billing Model** | **Per-second billing** | Flat hourly App Service Plan | Per VM worker node |
| **Best Enterprise Use**| **Batch ETL jobs, CI/CD runners, scheduled tasks** | Long-running web applications | Complex microservice architectures |

```bash
# Run a One-Off Batch ETL Job Container in ACI with Run-Once Policy
az container create   --resource-group demo-rg   --name batch-etl-pipeline   --image myacr.azurecr.io/etl-job:latest   --restart-policy Never   --environment-variables SOURCE_BLOB="raw/sales_2025.csv" DEST_TABLE="dw.sales"   --secure-environment-variables DB_PASSWORD="SecretPassword123"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Custom Domains SSL Certificates and VNet Integration](./03-Custom-Domains-SSL-Certificates-and-VNet-Integration.md) | [Index](../../../README.md) | [05 - Azure Functions Hosting Plans Consumption Premium Flex →](./05-Azure-Functions-Hosting-Plans-Consumption-Premium-Flex.md) |
