# 06 - EndpointSlices: High-Scale Service Endpoints

## 1. Why EndpointSlices Replaced Endpoints

In early Kubernetes, all Pod IPs for a Service were stored in a single monolithic `Endpoints` object.
When a Service scaled to 2,000 pods:
- Any single pod restart or IP change updated the entire 1.5MB Endpoints object.
- `kube-apiserver` had to transmit that 1.5MB payload to every worker node running `kube-proxy`.
- This saturated control plane bandwidth and choked etcd.

---

## 2. The EndpointSlice Architecture

**EndpointSlices** break endpoints into smaller chunks (default: **100 endpoints per slice**):
- Service with 2,500 pods = 25 distinct `EndpointSlice` objects.
- If Pod 42 restarts, only Slice #1 is updated and broadcast across the cluster!

```bash
# View endpoint slices
kubectl get endpointslices -l kubernetes.io/service-name=my-service
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Kube Proxy Modes iptables vs IPVS vs Kernel Routing](./05-Kube-Proxy-Modes-iptables-vs-IPVS-vs-Kernel-Routing.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
