# 05 - Azure Bastion, Private Endpoints & Private Link

> **Focus:** Zero-public-IP server management, Azure Bastion browser-based RDP/SSH jump host, Private Endpoints, and Private DNS Zone resolution.

---

## 🏰 1. Azure Bastion Architecture

**Azure Bastion** is a fully managed PaaS service provisioned inside your VNet that lets you connect to your virtual machines via the Azure Portal using your web browser over **HTTPS (Port 443)** without exposing any public IPs on your VMs:

```text
Administrator Web Browser
            │
            ▼ (Encrypted HTTPS on Port 443)
┌────────────────────────────────────────────────────────┐
│                  Azure Bastion Subnet                  │
│               (Name MUST BE: AzureBastionSubnet)       │
└───────────────────────────┬────────────────────────────┘
                            │ Private RDP (3389) or SSH (22) over VNet
                            ▼
┌────────────────────────────────────────────────────────┐
│                   Private Backend VM                   │
│        (NO Public IP! Zero exposure to internet!)       │
└────────────────────────────────────────────────────────┘
```

---

## 🔒 2. Private Endpoints & Azure Private Link

A **Private Endpoint** brings an Azure PaaS service (Azure SQL, Storage, Key Vault) directly into your private VNet by assigning it a private IP address from your subnet:

```text
Feature: Service Endpoint vs. Private Endpoint
• Service Endpoint: Traffic traverses Azure backbone to the public IP of the PaaS service (Free).
• Private Endpoint: The PaaS service receives a PRIVATE IP (e.g. 10.0.1.5) directly inside your subnet (Paid per hr/GB).
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Azure Route Tables and User Defined Routes UDR](./04-Azure-Route-Tables-and-User-Defined-Routes-UDR.md) | [Index](../../../README.md) | [06 - Hybrid Connectivity Azure VPN Gateway and ExpressRoute →](./06-Azure-Hybrid-Connectivity-Azure-VPN-Gateway-and-ExpressRoute.md) |
