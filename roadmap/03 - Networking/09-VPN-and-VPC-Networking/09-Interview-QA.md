# 09 - VPN & VPC: Interview Questions & Answers

### Q1: What is the difference between AWS Security Groups and Network ACLs (NACLs)?
**Answer:**
1. **Scope:** Security Groups operate at the instance/ENI level. NACLs operate at the subnet boundary.
2. **Statefulness:** Security Groups are **stateful** (return traffic automatically allowed). NACLs are **stateless** (inbound and outbound rules must be defined explicitly).
3. **Evaluation:** Security Groups evaluate all rules before deciding to allow. NACLs evaluate numbered rules in sequential order.
4. **Deny Rules:** NACLs support explicit DENY rules. Security Groups support only ALLOW rules.

### Q2: Why is VPC Peering non-transitive?
**Answer:** AWS deliberately restricts peering from routing traffic through intermediate VPCs to prevent complex routing loops, cross-account billing ambiguities, and noisy neighbor saturation of intermediate ENIs.

### Q3: Why does WireGuard outperform OpenVPN and IPsec?
**Answer:** WireGuard runs entirely in the Linux kernel (avoiding user/kernel context switches common in OpenVPN), uses modern streamlined cryptographic primitives (ChaCha20-Poly1305), and has a tiny codebase (~4,000 lines) optimized for modern multicore CPUs.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
