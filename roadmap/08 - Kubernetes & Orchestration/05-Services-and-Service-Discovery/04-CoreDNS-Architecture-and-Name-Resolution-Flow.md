# 04 - CoreDNS Architecture and Name Resolution Flow

## 1. How Pods Resolve DNS

Every container created in Kubernetes has its `/etc/resolv.conf` injected by Kubelet:

```text
nameserver 10.96.0.10             # CoreDNS ClusterIP
search default.svc.cluster.local svc.cluster.local cluster.local us-east-1.compute.internal
options ndots:5
```

---

## 2. The `ndots:5` Latency Penalty

Because `ndots:5` is default, if an application requests an external domain like `api.stripe.com` (which has only 2 dots), the Linux DNS resolver will first append every search domain in order:
1. `api.stripe.com.default.svc.cluster.local.` ──► NXDOMAIN (Query 1)
2. `api.stripe.com.svc.cluster.local.`         ──► NXDOMAIN (Query 2)
3. `api.stripe.com.cluster.local.`             ──► NXDOMAIN (Query 3)
4. `api.stripe.com.us-east-1.compute.internal.`──► NXDOMAIN (Query 4)
5. `api.stripe.com.`                          ──► SUCCESS  (Query 5!)

> [!TIP]
> **Performance Optimization:** Configure `spec.dnsConfig.options` with `ndots: 2` on egress-heavy microservices, or append a trailing dot (`api.stripe.com.`) in application code to bypass search paths!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Headless Services](./03-Headless-Services-and-Stateful-DNS-Discovery.md) | [README](./README.md) | [05 - Kube-Proxy Modes](./05-Kube-Proxy-Modes-iptables-vs-IPVS-vs-Kernel-Routing.md) |
