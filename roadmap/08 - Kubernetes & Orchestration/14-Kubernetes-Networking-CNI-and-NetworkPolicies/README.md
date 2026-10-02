# 14 - Kubernetes Networking, CNI, and NetworkPolicies

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Kubernetes-Networking-Model-and-IP-per-Pod-Rule.md` — Core networking tenets: IP-per-Pod, flat address space without NAT, and inter-node packet flow.
2. `02-Container-Network-Interface-CNI-Specification.md` — The CNI plugin contract: `ADD`, `DEL`, `CHECK` JSON commands, and IPAM (Host-Local vs DHCP).
3. `03-Calico-CNI-BGP-Routing-and-IP-Pools.md` — Enterprise L3 routing: Calico architecture (`Felix`, `BIRD`, `BGP`), IP Pools, and VXLAN / IPIP encapsulation.
4. `04-Cilium-CNI-eBPF-Datapath-and-High-Performance-Networking.md` — eBPF revolution: Cilium architecture, socket-level bypassing of Netfilter, Hubble observability, and XDP line-rate DDoS filtering.
5. `05-Native-NetworkPolicies-Ingress-Egress-and-Namespaces.md` — Zero-Trust micro-segmentation: Default Deny all, namespace selectors, pod selectors, and CIDR blocks.
6. `06-Advanced-L7-Network-Policies-and-DNS-Filtering.md` — Application-aware network policies: CiliumNetworkPolicy, FQDN egress whitelisting, and HTTP method/path filtering.
7. `07-Real-World-Scenarios.md` — Production post-mortems: MTU mismatch causing silent packet drops, and default-allow breach lateral movement.
8. `08-Troubleshooting.md` — Diagnostic decision tree for cross-node pod-to-pod ping failures and blocked DNS traffic.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Kubernetes networking and CNI plugins.
10. `10-Hands-On-Practice.md` — Production lab: implementing a strict Default Deny Zero-Trust NetworkPolicy with explicit DB and DNS egress rules.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with detailed explanations.
12. `12-Quick-Revision.md` — High-density CNI comparison and NetworkPolicy syntax cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 13 - Helm](../13-Helm-Package-Manager-and-Charts/README.md) | [README](./README.md) | [01 - Networking Model](./01-Kubernetes-Networking-Model-and-IP-per-Pod-Rule.md) |
