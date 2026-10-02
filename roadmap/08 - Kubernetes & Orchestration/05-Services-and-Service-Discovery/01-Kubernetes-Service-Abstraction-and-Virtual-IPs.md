# 01 - Kubernetes Service Abstraction and Virtual IPs

## 1. Why Do We Need Services?

Pods in Kubernetes are ephemeral. They are created and destroyed dynamically during scaling, updates, and node failures. Each Pod receives an ephemeral IP address.
A **Service** provides an immutable, stable IP address and DNS name that load-balances traffic across an ever-changing set of backend Pods.

```text
                               +----------------------------+
                               |     Client / Frontend      |
                               +--------------+-------------+
                                              | Requests to http://10.96.0.15:80
                                              v
                               +----------------------------+
                               |    Service: order-service  |
                               |    ClusterIP: 10.96.0.15   |
                               |    Port: 80 ──► Target:8080|
                               +--------------+-------------+
                                              |
                   ┌──────────────────────────┼──────────────────────────┐
                   v                          v                          v
          +-----------------+        +-----------------+        +-----------------+
          | Pod 1           |        | Pod 2           |        | Pod 3           |
          | IP: 10.244.1.12 |        | IP: 10.244.2.8  |        | IP: 10.244.3.19 |
          | Port: 8080      |        | Port: 8080      |        | Port: 8080      |
          +-----------------+        +-----------------+        +-----------------+
```

---

## 2. Virtual IP Mechanics

The **ClusterIP** is not a physical network interface! There is no network adapter on any machine with the IP `10.96.0.15`.
Instead, `kube-proxy` programs the Linux kernel's Netfilter/iptables or IPVS subsystem on every worker node to intercept packets directed to that VIP and rewrite the destination IP to one of the live Pod IPs (DNAT).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (04-Deployments-ReplicaSets-and-Rollouts)](../04-Deployments-ReplicaSets-and-Rollouts/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Service Types ClusterIP NodePort and LoadBalancer →](./02-Service-Types-ClusterIP-NodePort-and-LoadBalancer.md) |
