# 03 - Azure SDK for Python and Identity Libraries

## 1. `azure-identity` and `DefaultAzureCredential`

Modern Azure development mandates `azure-identity`. The `DefaultAzureCredential` class implements an opinionated authentication chain that allows the exact same code to run in local development, CI/CD runners, and Azure cloud environments without changing a single line of code.

```text
DefaultAzureCredential Authentication Evaluation Order:
  1. Environment Variables (AZURE_CLIENT_ID, AZURE_TENANT_ID, AZURE_CLIENT_SECRET)
  2. Workload Identity (Kubernetes service account tokens)
  3. Managed Identity (Azure VM, AKS pod, Container App, App Service)
  4. Azure CLI Token (`az login` developer credentials)
  5. Azure PowerShell Token (`Connect-AzAccount`)
```

```python
from azure.identity import DefaultAzureCredential
from azure.mgmt.compute import ComputeManagementClient
from azure.mgmt.resource import ResourceManagementClient
import os

# Zero hardcoded secrets!
credential = DefaultAzureCredential()
subscription_id = os.environ.get("AZURE_SUBSCRIPTION_ID", "00000000-0000-0000-0000-000000000000")

compute_client = ComputeManagementClient(credential, subscription_id)
resource_client = ResourceManagementClient(credential, subscription_id)
```

---

## 2. Handling Async Operations with `LROPoller` (Long-Running Operations)

In the Azure SDK, operations that take significant time (e.g., creating a VM, restarting an AKS cluster) return an **`LROPoller`** object.

```python
from azure.identity import DefaultAzureCredential
from azure.mgmt.compute import ComputeManagementClient

credential = DefaultAzureCredential()
subscription_id = "00000000-0000-0000-0000-000000000000"
compute_client = ComputeManagementClient(credential, subscription_id)

resource_group = "rg-prod-infrastructure"
vm_name = "vm-worker-01"

print(f"Triggering asynchronous restart of {vm_name}...")
async_poller = compute_client.virtual_machines.begin_restart(resource_group, vm_name)

# Non-blocking status inspection:
print(f"Operation status: {async_poller.status()}")

# Blocking wait for completion:
async_poller.result()
print(f"VM {vm_name} successfully restarted!")
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - AWS Boto3 Deep Dive Paginators Waiters and Config](./02-AWS-Boto3-Deep-Dive-Paginators-Waiters-and-Config.md) | [Index](../../../README.md) | [04 - Google Cloud Client Libraries and Service Accounts →](./04-Google-Cloud-Client-Libraries-and-Service-Accounts.md) |
