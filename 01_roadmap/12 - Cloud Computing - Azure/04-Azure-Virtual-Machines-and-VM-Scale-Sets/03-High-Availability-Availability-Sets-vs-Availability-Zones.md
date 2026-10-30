# 03 - High Availability: Availability Sets vs. Availability Zones

> **Focus:** IaaS resilience engineering, Fault Domains (FDs) vs. Update Domains (UDs), Availability Sets (99.95% SLA) vs. Availability Zones (99.99% SLA), and quorum architectures.

---

## 🏢 1. Availability Sets: Fault Domains vs. Update Domains

An **Availability Set** is a logical grouping of VMs within a single datacenter designed to prevent simultaneous hardware outages or maintenance reboots:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                     AVAILABILITY SET (Single Datacenter)               │
│                                                                        │
│   FAULT DOMAIN 0 (Rack 1)       FAULT DOMAIN 1 (Rack 2)                │
│   ├── VM 1 (Update Domain 0)    ├── VM 2 (Update Domain 1)             │
│   └── Independent Power/Network └── Independent Power/Network          │
└────────────────────────────────────────────────────────────────────────┘
```

- **Fault Domain (FD):** Represents physical hardware, a server rack, power supply, and network switch. Azure provides up to **3 Fault Domains**.
- **Update Domain (UD):** Represents a group of VMs that can be rebooted at the same time during planned hypervisor maintenance. Azure provides up to **20 Update Domains**.

---

## 📊 2. Availability Sets vs. Availability Zones

| Resiliency Architecture | Physical Scope | Protection Level | Uptime SLA |
| :--- | :--- | :--- | :---: |
| **Single VM (Premium SSD)**| 1 Host Server | Protects against individual drive failure via 3x disk copies. | **`99.9%`** |
| **Availability Set** | 1 Datacenter Building | Protects against single server rack and hypervisor maintenance reboots. | **`99.95%`** |
| **Availability Zones** | 3 Separate Datacenters | Protects against full datacenter building failure (flooding, power blackout). | **`99.99%`** |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Azure Managed Disks Ultra Premium Standard SSD](./02-Azure-Managed-Disks-Ultra-Premium-Standard-SSD.md) | [Index](../../../README.md) | [04 - Virtual Machine Scale Sets VMSS and Autoscaling →](./04-Virtual-Machine-Scale-Sets-VMSS-and-Autoscaling.md) |
