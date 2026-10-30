# 05 - DaemonSet Architecture and Node-Level Agents

## 1. What Is a DaemonSet?

A **DaemonSet** is a specialized Kubernetes workload controller that guarantees that all (or a filtered subset of) worker nodes in a cluster run exactly **one copy of a designated Pod**. As new worker nodes are joined to the cluster (for example, via an Auto-Scaling Group or Karpenter), the DaemonSet controller automatically detects the new hardware and schedules a pod instance onto it. Conversely, when a node is removed or terminated from the cluster, the DaemonSet pods are automatically garbage collected.

While `Deployments` and `ReplicaSets` are designed for application microservices where pod placement is fungible and driven by resource availability, DaemonSets are designed for **infrastructure-level node agents**. These agents provide foundational system capabilities directly to the host operating system, such as log harvesting, hardware telemetry, kernel-level eBPF packet routing, and intrusion detection.

```text
+---------------------------------------------------------------------------------------------------+
|                        DAEMONSET AUTOMATIC NODE-LEVEL RECONCILIATION                              |
+---------------------------------------------------------------------------------------------------+

 Cluster Topology:
 ┌───────────────────────────┐   ┌───────────────────────────┐   ┌───────────────────────────┐
 │ WORKER NODE 01            │   │ WORKER NODE 02            │   │ WORKER NODE 03 (New Node) │
 │  ┌─────────────────────┐  │   │  ┌─────────────────────┐  │   │  ┌─────────────────────┐  │
 │  │ Fluentbit Pod       │  │   │  │ Fluentbit Pod       │  │   │  │ DaemonSet Controller│  │
 │  ├─────────────────────┤  │   │  ├─────────────────────┤  │   │  │ Automatically Boots │  │
 │  │ Node-Exporter Pod   │  │   │  │ Node-Exporter Pod   │  │   │  │ Fluentbit + Exporter│  │
 │  ├─────────────────────┤  │   │  ├─────────────────────┤  │   │  └─────────────────────┘  │
 │  │ Cilium CNI eBPF Pod │  │   │  │ Cilium CNI eBPF Pod │  │   │  Newly joined instance    │
 │  └─────────────────────┘  │   │  └─────────────────────┘  │   │  automatically covered!   │
 └───────────────────────────┘   └───────────────────────────┘   └───────────────────────────┘
```

---

## 2. Core Operational Use Cases for DaemonSets

Infrastructure engineers rely on DaemonSets for four primary operational pillars:

1. **Host and Container Log Harvesters:** Tools like Fluentbit, Fluentd, Promtail, and Vector run as DaemonSets with read-only `hostPath` volume mounts to `/var/log` and `/var/lib/docker/containers/`. They tail raw container stdout/stderr log files, enrich them with Kubernetes metadata (namespace, pod name, labels), and forward them to Elasticsearch, Loki, or Datadog.
2. **Hardware and Kernel Metric Exporters:** Prometheus `node-exporter` requires direct visibility into host CPU, memory, disk I/O, and network socket statistics by mounting the host `/proc` and `/sys` virtual filesystems.
3. **Container Network Interface (CNI) Agents:** Modern CNI plugins such as Cilium, Calico, and AWS VPC CNI run a local routing agent on every single node. The agent configures Linux network namespaces, manipulates `iptables`/`IPVS` tables, and attaches eBPF bytecode to the host network interfaces (`eth0`).
4. **Endpoint Security and Runtime Threat Detection:** Security agents like Falco, Sysdig, CrowdStrike Falcon, and Datadog Security Agent monitor Linux kernel syscalls via eBPF or kernel modules to detect unauthorized bash shells, privilege escalation, or container escapes in real time.

---

## 3. How DaemonSets Schedule Pods: The Kube-Scheduler Evolution

Historically (prior to Kubernetes 1.12), the DaemonSet controller bypassed the default Kubernetes scheduler entirely by directly writing the `spec.nodeName` field into the Pod manifest. This caused severe operational bugs: DaemonSet pods ignored node taints, affinity rules, and priority classes.

In modern Kubernetes, the **DaemonSet controller cooperates directly with `kube-scheduler`**:
1. When a new node is registered, the DaemonSet controller creates a Pod object with a pre-configured `NodeAffinity` targeting that specific node:
   ```yaml
   affinity:
     nodeAffinity:
       requiredDuringSchedulingIgnoredDuringExecution:
         nodeSelectorTerms:
           - matchFields:
               - key: metadata.name
                 operator: In
                 values: ["worker-node-03"]
   ```
2. The `kube-scheduler` binds the pod to the node while honoring cluster-wide scheduling policies.

```text
+---------------------------------------------------------------------------------------------------+
|                        DAEMONSET SCHEDULING AND RECONCILIATION FLOW                               |
+---------------------------------------------------------------------------------------------------+

 1. Node Joined ──► 2. DaemonSet Controller ──► 3. Creates Unbound Pod ──► 4. Kube-Scheduler
    (Event seen     Detects missing pod on         with NodeAffinity for      Evaluates Taints,
     in etcd)       Node "worker-03"               Node "worker-03"           Priority, & Binds!
```

---

## 4. Scheduling on Control Plane Nodes via Tolerations

By default, Kubernetes control plane nodes (master nodes) are tainted to prevent application workloads from consuming control plane resources:
```text
node-role.kubernetes.io/control-plane:NoSchedule
```
However, foundational infrastructure DaemonSets (such as CNI network routers and Prometheus node exporters) must run on control plane nodes to ensure cluster networking and monitoring function properly. To allow DaemonSet pods onto control plane nodes, explicit **tolerations** must be declared:

```yaml
tolerations:
  - key: node-role.kubernetes.io/control-plane
    operator: Exists
    effect: NoSchedule
  - key: node-role.kubernetes.io/master
    operator: Exists
    effect: NoSchedule
```

---

## 5. Production Manifest: Prometheus Node-Exporter DaemonSet

The following manifest illustrates a production-grade DaemonSet leveraging host namespaces, read-only system mounts, and control-plane tolerations:

```yaml
apiVersion: apps/v1
kind: DaemonSet
metadata:
  name: node-exporter
  namespace: monitoring
  labels:
    app.kubernetes.io/name: node-exporter
spec:
  selector:
    matchLabels:
      app.kubernetes.io/name: node-exporter
  template:
    metadata:
      labels:
        app.kubernetes.io/name: node-exporter
    spec:
      hostNetwork: true
      hostPID: true
      tolerations:
        - operator: Exists
          effect: NoSchedule
      containers:
        - name: node-exporter
          image: quay.io/prometheus/node-exporter:v1.7.0
          args:
            - "--path.procfs=/host/proc"
            - "--path.sysfs=/host/sys"
            - "--path.rootfs=/host/root"
          ports:
            - containerPort: 9100
              hostPort: 9100
              name: metrics
          resources:
            requests:
              cpu: 50m
              memory: 64Mi
            limits:
              cpu: 200m
              memory: 128Mi
          securityContext:
            readOnlyRootFilesystem: true
          volumeMounts:
            - name: proc
              mountPath: /host/proc
              readOnly: true
            - name: sys
              mountPath: /host/sys
              readOnly: true
            - name: root
              mountPath: /host/root
              readOnly: true
      volumes:
        - name: proc
          hostPath:
            path: /proc
        - name: sys
          hostPath:
            path: /sys
        - name: root
          hostPath:
            path: /
```

---

## 6. Update Strategies: `RollingUpdate` vs `OnDelete`

DaemonSets support two distinct update strategies defined under `spec.updateStrategy.type`:

| Update Strategy | Operational Behavior | Failure Risk & Use Case |
| :--- | :--- | :--- |
| **`RollingUpdate` (Default)** | Gradually terminates old DaemonSet pods and boots new versions on nodes one by one. Governed by `maxUnavailable` (default: 1). | **Safe and automated.** Ideal for loggers and monitoring agents where temporary one-node downtime is acceptable. |
| **`OnDelete`** | When the manifest is updated, old pods remain running untouched. The new pod template is applied only when an operator manually runs `kubectl delete pod` on that node. | **Manual control.** Crucial for mission-critical CNI network agents (e.g. Cilium/Calico) to prevent a broken update from severing network connectivity across the entire cluster simultaneously. |

---

## 7. Operational Troubleshooting and Failure Modes

### Scenario A: DaemonSet Pod Stuck in `Pending` on Specific Nodes
- **Symptom:** DaemonSet reports desired replicas: 10, current: 8. Two nodes do not have pods.
- **Root Cause:** The two missing nodes have custom taints (such as `dedicated=gpu:NoSchedule` or `node.kubernetes.io/disk-pressure:NoSchedule`) that the DaemonSet pod manifest lacks tolerations for, or the node has insufficient CPU/memory to satisfy the pod's `requests`.
- **Diagnosis:**
  ```bash
  kubectl get nodes
  kubectl describe node <affected-node-name> | grep Taints
  ```

### Scenario B: Port Collision with `hostPort`
- **Symptom:** Pod fails to schedule with `Warning FailedScheduling: 0/10 nodes available: 1 node(s) didn't have free ports for the requested pod, 9 node(s) had untolerated taint`.
- **Root Cause:** The DaemonSet specifies a `hostPort: 9100`, but a rogue host process or another container running on that node is already bound to port 9100.
- **Remediation:** Inspect node network listeners with `ss -tulpn | grep 9100` and stop conflicting host processes.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Clustered Database Deployment Patterns MySQL Postgres](./04-Clustered-Database-Deployment-Patterns-MySQL-Postgres.md) | [Index](../../../README.md) | [06 - DaemonSet Update Strategies and HostPort Considerations →](./06-DaemonSet-Update-Strategies-and-HostPort-Considerations.md) |
