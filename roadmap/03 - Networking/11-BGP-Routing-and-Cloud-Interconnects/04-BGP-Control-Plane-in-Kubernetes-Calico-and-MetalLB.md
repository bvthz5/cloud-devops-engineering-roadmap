# 04 - BGP Control Plane in Kubernetes: Calico and MetalLB

## 1. Why Run BGP Inside Kubernetes?

In traditional Kubernetes on cloud VMs, overlay networks (VXLAN/Geneve) encapsulate Pod traffic inside UDP packets, incurring 50 bytes of overhead and CPU packet processing penalties.

In bare-metal enterprise Kubernetes:
- Every Kubernetes worker node runs a BGP routing daemon (**BIRD** inside Calico).
- Nodes peer directly with datacenter **Top-of-Rack (ToR) physical switches**.
- Every Pod receives a real, routable IP address on the corporate network without encapsulation!

```
                  [ Top-of-Rack Physical Switch ] (BGP Spine / Router)
                           /                  \
           eBGP Peering   /                    \   eBGP Peering
                         ▼                      ▼
               [ K8s Worker Node 1 ]       [ K8s Worker Node 2 ]
               Pod CIDR: 10.244.1.0/24     Pod CIDR: 10.244.2.0/24
```

---

## 2. MetalLB in BGP Mode

On bare metal, Kubernetes `type: LoadBalancer` services remain in `<pending>` state because there is no AWS/GCP cloud controller.
**MetalLB solves this:**
- MetalLB assigns a pool of public or corporate IPs (e.g. `198.51.100.0/28`).
- When a service is created, MetalLB establishes a BGP peering session with the upstream router and advertises a `/32` host route pointing to the nodes hosting the active Pods!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Cloud Interconnects](./03-Cloud-Dedicated-Interconnects-DirectConnect-and-ExpressRoute.md) | [README](./README.md) | [05 - BGP Security](./05-BGP-Security-RPKI-Route-Hijacking-and-Flap-Damping.md) |
