# 06 - Zero Trust Network Access (ZTNA) vs. Perimeter VPN

## 1. The Castle-and-Moat Vulnerability (Perimeter VPN)

In traditional corporate VPN architectures:
- The perimeter is fortified with firewalls and VPN concentrators.
- Once an employee connects to the VPN, they gain broad network-level Layer 3 access to entire subnets (`10.0.0.0/8`).
- **If an attacker compromises one laptop or VPN credential, they can scan, pivot, and execute lateral movement across all corporate servers!**

---

## 2. The Zero Trust Paradigm: "Never Trust, Always Verify"

**Zero Trust Network Access (ZTNA)** decouples access from network location:
- **Identity-Centric:** Access is granted to specific applications, never to raw network subnets.
- **Continuous Verification:** User identity, device compliance (antivirus, OS patch level), and location are evaluated per request.
- **Micro-segmentation:** Lateral movement is mathematically impossible because servers do not listen on open public or wide-area ports.

```
Traditional VPN:
[User Laptop] ────► [Corporate VPN] ────► [Direct Layer 3 Access to Entire Subnet: 10.0.0.0/16]

Zero Trust (Tailscale / Cloudflare Access):
[User Laptop] ────► [Identity Provider (Okta/Google)] ────► [Encrypted Proxy to App A ONLY]
                                                           (Database B is completely invisible)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Private Endpoints PrivateLink and PSC](./05-Private-Endpoints-PrivateLink-and-PSC.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
