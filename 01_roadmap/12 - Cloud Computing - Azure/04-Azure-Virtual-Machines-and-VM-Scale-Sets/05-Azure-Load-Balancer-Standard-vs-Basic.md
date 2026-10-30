# 05 - Azure Load Balancer Architecture: Standard vs. Basic SKU & Inbound NAT Rules

> **Focus:** Layer 4 TCP/UDP transport load balancing, 5-tuple hash distribution, Standard vs. Basic SKU differences, Health Probes, Load Balancing Rules, Inbound NAT rules, and Outbound SNAT rules.

---

## ⚖️ 1. Azure Load Balancer (Layer 4) Architecture

The **Azure Load Balancer** operates at **Layer 4 (Transport Layer)** of the OSI model. It distributes incoming traffic among healthy Virtual Machine instances in a backend address pool using a 5-tuple hash algorithm (`Source IP`, `Source Port`, `Destination IP`, `Destination Port`, `Protocol`).

```text
Internet
   │
   ▼
┌────────────────────────────────────────────────────────┐
│            Azure Standard Load Balancer (L4)           │
│  Frontend Public IP: 20.50.1.100 (Static / Zone-Redundant)
│                                                        │
│  Health Probe: TCP 80 (/healthcheck every 5s)         │
│  Load Balancing Rule: TCP 80 ➔ Backend Pool Port 80    │
│  Inbound NAT Rule: Port 50001 ➔ VM-1 SSH (Port 22)     │
└───────────────────────────┬────────────────────────────┘
                            │
              ┌─────────────┴──────────────┐
              ▼                            ▼
     ┌─────────────────┐          ┌─────────────────┐
     │      VM-1       │          │      VM-2       │
     │   (10.0.1.4)    │          │   (10.0.1.5)    │
     └─────────────────┘          └─────────────────┘
```

---

## 📊 2. Standard SKU vs. Basic SKU Comparison

| Architectural Feature | Basic SKU (Deprecated) | Standard SKU (Production Standard) |
| :--- | :--- | :--- |
| **Backend Pool Capacity** | Up to `300` VM instances. | Up to **`1,000`** VM instances. |
| **Availability Zones** | ❌ Not zone-redundant. | ✅ **Zone-redundant & zonal deployment supported**. |
| **SLA Availability** | ❌ No formal financial SLA. | ✅ **`99.99%`** uptime SLA. |
| **Default Security Posture**| ⚪ Open by default (Insecure). | 🛡️ **Secure by Default** (Requires explicit NSG rules). |
| **Health Probe Protocols** | HTTP, TCP. | **HTTP, HTTPS, TCP**. |
| **Inbound NAT Pools** | Basic NAT rules only. | Scalable Inbound NAT rules with port reuse. |
| **Pricing Model** | Free. | Hourly gateway fee + per-GB data processed. |

> ⚠️ **Critical Security Exam Rule:** VMs behind a **Standard Load Balancer** cannot receive traffic or pass health checks unless their Subnet/NIC NSG explicitly contains a rule permitting inbound traffic from the `AzureLoadBalancer` service tag or frontend IP!

---

## 💻 3. Complete Azure CLI Load Balancer Provisioning

```bash
# 1. Create Zone-Redundant Static Public IP
az network public-ip create   --resource-group rg-networking   --name pip-lb-prod   --sku Standard   --allocation-method Static   --zone 1 2 3

# 2. Create Standard Load Balancer
az network lb create   --resource-group rg-networking   --name lb-prod   --sku Standard   --frontend-ip-name fe-config   --public-ip-address pip-lb-prod   --backend-pool-name backend-pool

# 3. Create Health Probe (Probes /healthcheck every 5 seconds)
az network lb probe create   --resource-group rg-networking   --lb-name lb-prod   --name probe-http   --protocol Http   --port 80   --path /healthcheck   --interval 5   --threshold 2

# 4. Create Load Balancing Rule
az network lb rule create   --resource-group rg-networking   --lb-name lb-prod   --name rule-http   --protocol Tcp   --frontend-port 80   --backend-port 80   --frontend-ip-name fe-config   --backend-pool-name backend-pool   --probe-name probe-http   --idle-timeout 4   --enable-tcp-reset true

# 5. Add Inbound NAT Rule (Port 50001 maps directly to SSH on VM-1)
az network lb inbound-nat-rule create   --resource-group rg-networking   --lb-name lb-prod   --name nat-ssh-vm1   --protocol Tcp   --frontend-port 50001   --backend-port 22   --frontend-ip-name fe-config
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Virtual Machine Scale Sets VMSS and Autoscaling](./04-Virtual-Machine-Scale-Sets-VMSS-and-Autoscaling.md) | [Index](../../../README.md) | [06 - Azure Application Gateway Layer 7 and WAF →](./06-Azure-Application-Gateway-Layer-7-and-WAF.md) |
