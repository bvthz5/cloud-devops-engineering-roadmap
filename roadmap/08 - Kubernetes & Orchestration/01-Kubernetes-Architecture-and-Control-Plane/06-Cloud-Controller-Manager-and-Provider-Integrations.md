# 06 - Cloud Controller Manager and Provider Integrations

## 1. Why Cloud Controller Manager (CCM)?

Prior to Kubernetes v1.11, cloud provider code (AWS, Azure, GCP, OpenStack) was compiled directly inside `kube-controller-manager` (known as "in-tree"). This slowed down releases and forced cloud-specific dependencies into core Kubernetes.

The **Cloud Controller Manager (CCM)** decouples core Kubernetes from cloud APIs by running cloud-specific control loops:
- `Node Controller`: Checks with cloud provider if a stopped VM has been deleted in AWS/Azure/GCP.
- `Route Controller`: Configures underlying cloud VPC routing tables for Pod CIDRs.
- `Service Controller`: Dynamically provisions cloud Layer 4 load balancers (e.g., AWS NLB, Azure Standard LB) when a Service of `type: LoadBalancer` is created.

```text
+-----------------------+           +-----------------------+
| kube-controller-mgr   |           | cloud-controller-mgr  |
| - ReplicaSet          |           | - Cloud Node Reaper   |
| - Endpoints           |           | - Cloud VPC Routes    |
| - Namespaces          |           | - Cloud LoadBalancers |
+-----------+-----------+           +-----------+-----------+
            |                                   |
            +-----------------+-----------------+
                              v
                    +--------------------+
                    |   kube-apiserver   |
                    +--------------------+
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - HA Control Plane Topologies](./05-High-Availability-Control-Plane-Topologies.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
