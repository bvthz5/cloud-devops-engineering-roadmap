# 04 - CoreDNS Architecture and Name Resolution Flow

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to CoreDNS in Kubernetes, detailing `/etc/resolv.conf` injection, FQDN resolution mechanics, the infamous `ndots:5` latency penalty, the Corefile plugin pipeline, and NodeLocal DNSCache optimization.

---

## 🏛️ 1. What Is CoreDNS & How Does Kubernetes Use It?

In an enterprise cloud-native cluster hosting thousands of microservices, applications do not communicate using ephemeral IP addresses; they connect using human-readable domain names (such as `payment-service` or `auth-api.security`).

**CoreDNS** is the official, default DNS server for Kubernetes (graduated under the CNCF in 2019, replacing legacy `kube-dns`). It is written in Go and designed around a high-performance, modular **plugin architecture**.

In Kubernetes, CoreDNS runs as a standard Deployment in the `kube-system` namespace. It watches the Kubernetes API server for additions, updates, and deletions of Services and Pods, maintaining an in-memory, real-time authoritative DNS zone for the cluster (defaulting to the `.cluster.local` root domain).

---

## ⚙️ 2. How Pods Resolve DNS: The `/etc/resolv.conf` Injection

When the `kubelet` initializes a Pod on a worker node, it automatically generates and injects a dedicated `/etc/resolv.conf` file directly into the container's filesystem:

```text
nameserver 10.96.0.10
search default.svc.cluster.local svc.cluster.local cluster.local us-east-1.compute.internal
options ndots:5
```

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   NAME RESOLUTION PROGRESSION MODEL                    │
└────────────────────────────────────────────────────────────────────────┘

 1. Short Name Query (Same Namespace):
    curl http://database
    ├── Linux resolver checks search domains in /etc/resolv.conf
    └── Appends first domain: database.default.svc.cluster.local. ──► RESOLVED!

 2. Cross-Namespace Query:
    curl http://database.production
    ├── Appends first domain: database.production.default.svc.cluster.local. (NXDOMAIN)
    └── Appends second domain: database.production.svc.cluster.local. ──► RESOLVED!

 3. Fully Qualified Domain Name (FQDN):
    curl http://database.production.svc.cluster.local.
    └── Trailing dot indicates root domain; resolves IMMEDIATELY on first attempt!
```

---

## 💥 3. The Infamous `ndots:5` Latency Penalty

The single greatest source of hidden network latency and CoreDNS CPU exhaustion in production clusters is the **`ndots:5`** default option.

### What Does `ndots` Mean?
In Linux POSIX DNS resolvers, the `ndots` parameter defines the threshold of dots (periods) a query must contain to be considered an absolute domain name:
- If a queried hostname contains **fewer dots than `ndots`**, the Linux resolver will **first append every search domain in order** before attempting to resolve the raw hostname!

### The External Query Explosion:
Consider an application making an HTTPS request to an external cloud API, such as `api.stripe.com` (which contains only **2 dots**):

Because $2 < 5$, the container's resolver refuses to query `api.stripe.com` directly. Instead, it fires **five sequential DNS queries**:

1. `api.stripe.com.default.svc.cluster.local.` $\longrightarrow$ **NXDOMAIN** (Query 1: Wasted round-trip!)
2. `api.stripe.com.svc.cluster.local.` $\longrightarrow$ **NXDOMAIN** (Query 2: Wasted round-trip!)
3. `api.stripe.com.cluster.local.` $\longrightarrow$ **NXDOMAIN** (Query 3: Wasted round-trip!)
4. `api.stripe.com.us-east-1.compute.internal.` $\longrightarrow$ **NXDOMAIN** (Query 4: Wasted round-trip!)
5. `api.stripe.com.` $\longrightarrow$ **SUCCESS** (Query 5: Finally resolves after 4 failed queries!)

In a high-throughput microservice processing 10,000 requests per second, **a single external API call triggers 40,000 wasted NXDOMAIN queries per second**, saturating CoreDNS replicas, causing UDP packet drops, and introducing 5-second connection timeouts!

---

## 🚀 4. How to Fix the `ndots:5` Latency Penalty

DevOps engineers eliminate this query explosion using three enterprise optimization strategies:

### Strategy 1: Tuning `dnsConfig` in the PodSpec (Recommended)
You can override `ndots` per workload by declaring a custom `dnsConfig` in your Deployment manifest. Setting `ndots: 2` ensures that any domain with 2 or more dots (such as `api.stripe.com`) is resolved directly against public upstream DNS on the first attempt:

```yaml
spec:
  template:
    spec:
      dnsConfig:
        options:
        - name: ndots
          value: "2"
```

### Strategy 2: Appending a Trailing Dot in Application Code
Instruct your application developers to append an explicit trailing dot to external URLs (e.g., `https://api.stripe.com./v1/charges`). The trailing dot explicitly marks the hostname as a Fully Qualified Domain Name (FQDN), informing the Linux resolver that it belongs to the root DNS zone and completely bypassing all search domains!

### Strategy 3: Deploying NodeLocal DNSCache
In large-scale clusters, operators deploy **NodeLocal DNSCache** as a DaemonSet. It runs a lightweight caching agent listening on a local link-local IP (`169.254.20.10`) on every worker node, caching DNS responses locally in memory and converting UDP queries into persistent TCP connections to CoreDNS, eliminating packet drops.

---

## 🔬 5. Inside the CoreDNS Corefile Configuration

CoreDNS behavior is configured declaratively through a ConfigMap named `coredns` in the `kube-system` namespace. The configuration uses a structured DSL called the **Corefile**:

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: coredns
  namespace: kube-system
data:
  Corefile: |
    .:53 {
        errors                    # Log all query errors to stdout
        health {
            lameduck 5s           # Grace period during shutdown
        }
        ready                     # Reports HTTP 200 on port 8181 for readiness probe
        kubernetes cluster.local in-addr.arpa ip6.arpa {
            pods insecure
            fallthrough in-addr.arpa ip6.arpa
            ttl 30
        }
        prometheus :9153          # Exposes Prometheus metrics on port 9153
        forward . /etc/resolv.conf # Forwards external queries to host upstream DNS
        cache 30                  # Caches responses in RAM for up to 30 seconds
        loop                      # Detects and breaks infinite DNS routing loops
        reload                    # Hot-reloads Corefile without restarting process
        loadbalance               # Round-robins multi-A record response orders
    }
```

---

## 🛠️ 6. Troubleshooting DNS Failures in Real Time

```bash
# 1. Check health and replica count of CoreDNS deployment
kubectl get deployment coredns -n kube-system

# 2. View real-time CoreDNS logs to identify query timeouts or forwarding errors
kubectl logs -n kube-system -l k8s-app=kube-dns -f --tail=100

# 3. Spin up an interactive ephemeral network diagnostic pod to test DNS resolution
kubectl run dns-test --rm -it --image=infoblox/dnstools --restart=Never

# Inside the test pod:
# Test internal service resolution
dig order-service.default.svc.cluster.local

# Test external resolution and trace search domain traversal
nslookup -debug api.stripe.com
```

---

## 📋 7. Summary and Navigation

CoreDNS provides the transparent name resolution engine that makes Kubernetes service discovery intuitive and dynamic. By understanding `/etc/resolv.conf` injection, mitigating the `ndots:5` query multiplication penalty, and tuning the Corefile plugin pipeline, infrastructure architects ensure low-latency, resilient DNS across high-throughput enterprise clusters.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Headless Services and Stateful DNS Discovery](./03-Headless-Services-and-Stateful-DNS-Discovery.md) | [Index](../../../README.md) | [05 - Kube Proxy Modes iptables vs IPVS vs Kernel Routing →](./05-Kube-Proxy-Modes-iptables-vs-IPVS-vs-Kernel-Routing.md) |
