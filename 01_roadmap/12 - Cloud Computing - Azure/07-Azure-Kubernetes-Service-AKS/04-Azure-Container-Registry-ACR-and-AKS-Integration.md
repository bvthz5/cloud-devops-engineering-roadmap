# 04 - Azure Container Registry (ACR) & Native AKS Integration

> **Focus:** Private Docker image registry, SKUs (Basic, Standard, Premium), automated security scanning via Defender, and passwordless `--attach-acr` integration.

---

## 📦 1. Azure Container Registry (ACR) Architecture

```bash
# 1. Create Azure Container Registry (Standard SKU)
az acr create \
  --resource-group rg-containers \
  --name contosoregistryprod \
  --sku Standard

# 2. Attach ACR to AKS without managing image pull secrets!
# (Grants AcrPull role to AKS Kubelet Managed Identity automatically)
az aks update \
  --resource-group rg-containers \
  --name aks-production-cluster \
  --attach-acr contosoregistryprod
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - AKS Authentication Entra ID Integration and Workload Identity](./03-AKS-Authentication-Entra-ID-Integration-and-Workload-Identity.md) | [Index](../../../README.md) | [05 - AKS Storage Azure Disks Azure Files and CSI Drivers →](./05-AKS-Storage-Azure-Disks-Azure-Files-and-CSI-Drivers.md) |
