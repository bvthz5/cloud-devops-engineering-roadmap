# 04 - Azure Container Registry (ACR) Integration

Attach ACR to AKS cluster for seamless image pulling:
```bash
az aks update -n myAKSCluster -g myResourceGroup --attach-acr myACRRegistry
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - AKS Authentication Entra ID Integration and Workload Identity](./03-AKS-Authentication-Entra-ID-Integration-and-Workload-Identity.md) | [Index](../../../README.md) | [05 - AKS Storage Azure Disks Azure Files and CSI Drivers →](./05-AKS-Storage-Azure-Disks-Azure-Files-and-CSI-Drivers.md) |
