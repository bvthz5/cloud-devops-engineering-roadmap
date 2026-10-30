# 04 - VPC Peering vs. AWS Transit Gateway (Hub-and-Spoke Architecture)

> **Focus:** Inter-VPC connectivity, VPC Peering mesh limitations, Non-transitive routing constraints, and AWS Transit Gateway centralized routing.

---

## 🕸️ 1. VPC Peering vs. AWS Transit Gateway

```text
VPC Peering Mesh (Complex: N*(N-1)/2 Links):     Transit Gateway (Hub-and-Spoke: Clean & Scalable):
     ┌─────────┐                                           ┌─────────┐
     │  VPC A  ├───────────────┐                           │  VPC A  │
     └──┬───┬──┘               │                           └───┬─────┘
        │   │                  │                               │ Attachment
        │   └────────┐         │                               ▼
        ▼            ▼         ▼                     ┌──────────────────┐
   ┌─────────┐   ┌─────────┐ ┌─────────┐             │   AWS Transit    │
   │  VPC B  ├──►│  VPC C  │ │  VPC D  │             │     Gateway      │
   └─────────┘   └─────────┘ └─────────┘             └─────────┬────────┘
  (Non-transitive! If A ↔ B & B ↔ C, A cannot                  ▲ Attachment
   talk to C without an explicit A ↔ C link!)                  │
                                                           ┌───┴─────┐
                                                           │  VPC B  │
                                                           └─────────┘
```

### 📊 Comparison Table:

| Architectural Feature | VPC Peering | AWS Transit Gateway (TGW) |
| :--- | :--- | :--- |
| **Topology** | Point-to-point mesh. | Centralized **Hub-and-Spoke**. |
| **Transitive Routing**| ❌ **Non-Transitive** (No hop-through). | ✅ **Fully Transitive** across all attached VPCs and VPNs. |
| **Scale Limit** | High operational overhead beyond 10 VPCs. | Connects up to **5,000 VPCs** and on-premise networks. |
| **Data Processing Cost**| Standard inter-AZ/cross-region data transfer only. | Hourly fee per attachment + per-GB data processing fee. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Route Tables Security Groups and Network ACLs NACLs](./03-Route-Tables-Security-Groups-and-Network-ACLs-NACLs.md) | [Index](../../../README.md) | [05 - VPC Endpoints PrivateLink and VPC Flow Logs →](./05-VPC-Endpoints-PrivateLink-and-VPC-Flow-Logs.md) |
