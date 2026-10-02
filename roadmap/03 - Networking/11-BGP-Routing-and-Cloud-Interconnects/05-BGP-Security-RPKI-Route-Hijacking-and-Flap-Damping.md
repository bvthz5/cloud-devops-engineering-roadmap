# 05 - BGP Security: RPKI, Route Hijacking, and Flap Damping

## 1. The Fatal Flaw of BGP: Implicit Trust

BGP was designed in 1989 without cryptographic verification. By default, if any rogue ISP or compromised router advertises that it owns `1.1.1.1/32` or a more specific prefix like `8.8.8.0/25`, **global routers will forward traffic to the attacker due to longest-prefix match!**

Notable Incidents:
- **YouTube Hijack (2008):** Pakistan Telecom accidentally hijacked YouTube's entire global IP space.
- **Amazon Route 53 Hijack (2018):** Russian ISP hijacked AWS DNS IPs to steal cryptocurrency from MyEtherWallet users.

---

## 2. RPKI (Resource Public Key Infrastructure)

RPKI provides cryptographic proof of IP prefix ownership:
- A Regional Internet Registry (RIR) issues a **Route Origin Authorization (ROA)** signed with the owner's private key.
- The ROA declares: *"Only ASN 13335 (Cloudflare) is authorized to originate 1.1.1.0/24 with max length /24"*.
- Global routers validate ROAs and drop invalid routes (`RPKI Invalid = DROP`).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Kubernetes BGP](./04-BGP-Control-Plane-in-Kubernetes-Calico-and-MetalLB.md) | [README](./README.md) | [06 - OSPF vs BGP](./06-Dynamic-Routing-Protocols-OSPF-vs-BGP.md) |
