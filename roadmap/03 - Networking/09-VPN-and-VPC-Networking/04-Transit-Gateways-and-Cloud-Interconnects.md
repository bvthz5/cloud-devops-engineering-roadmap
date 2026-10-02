# 04 - Transit Gateways and Cloud Interconnects

## 1. What is a Transit Gateway (TGW)?

An **AWS Transit Gateway (TGW)** or **Azure Virtual WAN** acts as a regional virtual cloud router. Instead of creating hundreds of point-to-point mesh peerings, all VPCs, on-premises VPNs, and Direct Connect links attach directly to the Transit Gateway hub.

```
       [ Production VPC ]        [ Staging VPC ]        [ Shared Services VPC ]
       (10.10.0.0/16)            (10.20.0.0/16)         (10.30.0.0/16)
              │                         │                      │
              ▼                         ▼                      ▼
    ═════════════════════════════════════════════════════════════════════════
                            AWS TRANSIT GATEWAY (TGW)
                            (Regional Virtual Router)
    ═════════════════════════════════════════════════════════════════════════
              ▲                                                ▲
              │                                                │
       [ IPsec VPN ]                                  [ AWS Direct Connect ]
              │                                                │
    [ Corporate Branch Office ]                        [ On-Premises Data Center ]
```

---

## 2. Route Table Domains and Isolation

Transit Gateways support multiple isolated route tables:
- **Production Route Table:** Attached to Prod VPCs and Direct Connect. Cannot route to Dev/Staging.
- **Development Route Table:** Isolated from Prod; can only route to Dev VPCs and Shared Tools.
- **Inspection Route Table:** Funnels all traffic through a centralized firewall appliance (Palo Alto / Fortinet / AWS Network Firewall) before forwarding.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - VPC Peering](./03-VPC-Peering-Architecture-and-Limitations.md) | [README](./README.md) | [05 - PrivateLink & Endpoints](./05-Private-Endpoints-PrivateLink-and-PSC.md) |
