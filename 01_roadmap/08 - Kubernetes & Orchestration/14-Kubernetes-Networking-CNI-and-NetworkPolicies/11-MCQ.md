# 11 - Multiple-Choice Assessment (MCQ)

### Question 1
Which CNI plugin uses eBPF to route packets in the kernel and can replace `kube-proxy` entirely?
- [ ] A) Flannel
- [x] B) Cilium
- [ ] C) Weave Net
- [ ] D) Bridge CNI

<details>
<summary>Explanation</summary>
Cilium uses eBPF programs loaded directly into kernel hooks, eliminating the overhead of iptables and kube-proxy.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
