# 02 - xDS Dynamic Configuration APIs

## 1. The Universal Data Plane API (xDS)

In static configurations, modifying routes or backends requires restarting or reloading proxies. In modern service meshes, backend endpoints churn continuously as pods autoscale.

Envoy solves this through **xDS (Discovery Service APIs)**—a suite of gRPC streaming APIs that allow a central control plane (like Istio `istiod`) to dynamically stream configuration updates to thousands of Envoy proxies in real time without dropping a single packet:

```text
[ Central Control Plane (Istio / Envoy Gateway) ]
                       │
       ┌───────────────┼───────────────┬───────────────┐
       ▼ (gRPC Stream) ▼ (gRPC Stream) ▼ (gRPC Stream) ▼ (gRPC Stream)
     [ LDS ]         [ RDS ]         [ CDS ]         [ EDS ]
(Listener Disc.) (Route Disc.)   (Cluster Disc.) (Endpoint Disc.)
       │               │               │               │
       └───────────────┴───────┬───────┴───────────────┘
                               ▼
                       [ Envoy Proxy ]
```

### The 4 Core xDS APIs
1. **LDS (Listener Discovery Service)**: Dynamically defines ports and listener filter chains.
2. **RDS (Route Discovery Service)**: Dynamically defines HTTP route tables and virtual hosts.
3. **CDS (Cluster Discovery Service)**: Dynamically defines upstream services and balancing algorithms.
4. **EDS (Endpoint Discovery Service)**: Streams live IP addresses and ports of backend pods.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Envoy Architecture Threading and Event Model](./01-Envoy-Architecture-Threading-and-Event-Model.md) | [Index](../../../README.md) | [03 - Filter Chains and Network HTTP Filters →](./03-Filter-Chains-and-Network-HTTP-Filters.md) |
