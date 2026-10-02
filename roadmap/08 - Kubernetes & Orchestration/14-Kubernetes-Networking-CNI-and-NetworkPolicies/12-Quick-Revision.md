# 12 - Quick-Revision & Enterprise Cheat Sheet

## CNI & NetworkPolicy Summary

- **Core Rule:** Pods communicate without NAT; IP on pod matches destination IP.
- **Calico:** Layer 3 BGP routing + iptables/eBPF.
- **Cilium:** Next-gen eBPF datapath + Hubble observability + L7 FQDN filtering.
- **Default Policy:** Allow All (unless NetworkPolicy is applied).
- **MTU Sizing:** Ensure overlay (VXLAN: -50B) matches physical network MTU.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 15 - CRDs & Operators](../15-CRDs-and-Kubernetes-Operators/README.md) |
