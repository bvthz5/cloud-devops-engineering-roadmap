# 05 - Native NetworkPolicies: Ingress, Egress, and Namespaces

## 1. What Is a Kubernetes NetworkPolicy?

A **NetworkPolicy** is a Kubernetes-native resource that functions as a distributed, stateful layer 3 and layer 4 packet firewall. NetworkPolicies allow cluster administrators and developers to declare declarative rules specifying which network connections a Pod is permitted to accept (Ingress) and which external destinations it is permitted to initiate connections to (Egress).

### The Inherent Security Risk: The Default Flat Open Network
By default, **Kubernetes networks are non-isolated**. In a vanilla Kubernetes cluster without active NetworkPolicies:
1. Every Pod can initiate direct TCP, UDP, or ICMP connections to every other Pod across the entire cluster, regardless of namespaces.
2. An untrusted, internet-facing public web frontend running in a `development` namespace can directly ping and query a mission-critical PostgreSQL database running in a `production-finance` namespace.
3. If an attacker gains remote code execution (RCE) inside a single unprivileged container, they have unimpeded lateral movement across the entire cluster.

```text
+---------------------------------------------------------------------------------------------------+
|                        DEFAULT FLAT NETWORK VS ZERO-TRUST NETWORKPOLICIES                         |
+---------------------------------------------------------------------------------------------------+

 DEFAULT OPEN NETWORK (HIGH RISK):
 ┌──────────────────────┐                     ┌──────────────────────┐
 │ Public Web Frontend  │════════════════════►│ Production Database  │
 │ (Compromised by RCE) │  Direct TCP/5432    │ (postgres-0: 10.244) │
 └──────────────────────┘  Lateral Movement!  └──────────────────────┘

 ZERO-TRUST ENFORCED BY NETWORKPOLICY:
 ┌──────────────────────┐                     ┌──────────────────────┐
 │ Public Web Frontend  │───────X DROPPED X──►│ Production Database  │
 │ (Compromised by RCE) │  iptables / eBPF    │ (postgres-0)         │
 └──────────────────────┘  Denies Packet!     └──────────────────────┘
                                                        ▲
 ┌──────────────────────┐                               │
 │ Internal Backend API │═══════════════════════════════╝
 │ (Authenticated Tier) │  PERMITTED: Only authorized backend pods allowed!
 └──────────────────────┘
```

---

## 2. The CNI Enforcement Prerequisite

A critical architectural reality is that **the Kubernetes API server does not enforce NetworkPolicies**. When you apply a NetworkPolicy manifest, the API server simply validates and stores the object in `etcd`.

The actual enforcement of packet drops is entirely the responsibility of the underlying **Container Network Interface (CNI)** plugin.
- **Enforcing CNIs:** Cilium (via Linux kernel eBPF), Calico (via kernel `iptables` / `ipset` or eBPF), and Weave Net.
- **Non-Enforcing CNIs:** Standard **Flannel** does not have a policy engine. If you apply a NetworkPolicy in a Flannel cluster, `kubectl` reports success, but **no packets are ever blocked**.

---

## 3. The 3 Selector Types for Ingress and Egress Rules

Within a NetworkPolicy `ingress.from` or `egress.to` block, traffic can be filtered using three distinct selectors:

1. **`podSelector`:** Matches Pods by labels. If used alone, it matches pods *strictly within the same namespace* as the NetworkPolicy.
2. **`namespaceSelector`:** Matches all Pods running inside any namespace that carries matching labels.
3. **`ipBlock`:** Matches external IP CIDR ranges outside the cluster (e.g., enterprise subnets or external payment gateway APIs) and supports `except` exclusion lists.

```text
+---------------------------------------------------------------------------------------------------+
|                       THE CRITICAL YAML SYNTAX TRAP: AND vs OR LOGIC                              |
+---------------------------------------------------------------------------------------------------+

 EXAMPLE A: LOGICAL "AND" (Single List Item)
 ingress:
   - from:
     - namespaceSelector:
         matchLabels:
           environment: production
       podSelector:
         matchLabels:
           app: backend
 MEANING: Allows traffic ONLY from pods labeled "app: backend" that are inside namespaces labeled
          "environment: production".

 EXAMPLE B: LOGICAL "OR" (Two Separate List Items - Notice the Dash "-")
 ingress:
   - from:
     - namespaceSelector:
         matchLabels:
           environment: production
     - podSelector:
         matchLabels:
           app: backend
 MEANING: Allows traffic from ANY pod in "environment: production" namespaces OR from ANY pod
          labeled "app: backend" in the CURRENT namespace!
```

---

## 4. The Enterprise Zero-Trust Architecture Blueprint

In high-security production environments, organizations implement a **Default-Deny Zero-Trust Posture** across all namespaces using a three-tier policy pipeline:

### Step 1: Default Deny All Traffic in Namespace
This policy isolates every Pod in the namespace. Once applied, all incoming and outgoing connections are immediately dropped until explicitly allowlisted.

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: default-deny-all
  namespace: secure-workloads
spec:
  podSelector: {} # Selects all pods in this namespace
  policyTypes:
    - Ingress
    - Egress
```

### Step 2: Essential Core Egress: Allow DNS Resolution
When default deny egress is activated, containers cannot query CoreDNS. All internal DNS queries (e.g., resolving database service names) will fail with `Name resolution failure`. You must explicitly allow outbound UDP and TCP traffic on port 53:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-dns-egress
  namespace: secure-workloads
spec:
  podSelector: {}
  policyTypes:
    - Egress
  egress:
    - to:
        - namespaceSelector: {}
          podSelector:
            matchLabels:
              k8s-app: kube-dns
      ports:
        - protocol: UDP
          port: 53
        - protocol: TCP
          port: 53
```

### Step 3: Granular Microservice Allowlist: Database Ingress Policy
Explicitly allow incoming connections to the PostgreSQL database solely from authenticated backend microservices on port 5432:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: allow-postgres-ingress
  namespace: secure-workloads
spec:
  podSelector:
    matchLabels:
      app: postgres
  policyTypes:
    - Ingress
  ingress:
    - from:
        - podSelector:
            matchLabels:
              app: backend-api
      ports:
        - protocol: TCP
          port: 5432
```

---

## 5. Architectural Flow: How the Kernel Drops Unauthorized Packets

```text
+---------------------------------------------------------------------------------------------------+
|                        KERNEL-LEVEL PACKET FILTERING SEQUENCE (Cilium / Calico)                   |
+---------------------------------------------------------------------------------------------------+

 1. Unauthorized Packet Arrives at Worker Node NIC:
    Source: 10.244.1.80 (Rogue Pod) ──► Destination: 10.244.2.15:5432 (Postgres Pod)
         │
         ▼
 2. Linux Kernel eBPF XDP / tc Hook / iptables Raw Table:
    Inspects source IP and queries CNI identity cache.
    Source Identity: "app=untrusted-frontend, namespace=sandbox"
    Destination Identity: "app=postgres, namespace=secure-workloads"
         │
         ▼
 3. Policy Evaluation Engine:
    Does "allow-postgres-ingress" permit traffic from "app=untrusted-frontend"?
    NO MATCH FOUND!
         │
         ▼
 4. Immediate Packet Drop:
    Action: DROP (Packet silently discarded at kernel level).
    Latency Impact: Zero CPU overhead; packet never reaches user space socket buffers.
```

---

## 6. Operational Debugging and Connectivity Verification

```bash
# List all active network policies in a namespace
kubectl get networkpolicy -n secure-workloads

# Test network connectivity from inside a pod using netcat (nc)
kubectl exec -it frontend-pod -n secure-workloads -- nc -zv postgres-service 5432 -w 3

# If using Cilium CNI, monitor real-time packet drops and policy verdicts
cilium monitor --type drop

# If using Calico, inspect kernel ipsets generated for network policies
ipset list
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Cilium CNI eBPF Datapath and High Performance Networking](./04-Cilium-CNI-eBPF-Datapath-and-High-Performance-Networking.md) | [Index](../../../README.md) | [06 - Advanced L7 Network Policies and DNS Filtering →](./06-Advanced-L7-Network-Policies-and-DNS-Filtering.md) |
