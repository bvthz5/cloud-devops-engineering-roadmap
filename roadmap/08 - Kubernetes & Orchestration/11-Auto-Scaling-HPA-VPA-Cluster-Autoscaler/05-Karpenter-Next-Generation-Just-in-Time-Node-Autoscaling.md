# 05 - Karpenter: Next-Generation Just-in-Time Node Autoscaling

## 1. What Is Karpenter and Why Was It Created?

**Karpenter** is an open-source, high-performance, Kubernetes-native node autoscaling project (originally created by AWS, now under CNCF governance). It was designed to solve the deep architectural limitations of the legacy Kubernetes Cluster Autoscaler.

In traditional cloud Kubernetes architectures, the Cluster Autoscaler is fundamentally coupled to cloud provider **Auto Scaling Groups (ASGs)** or Managed Instance Groups (MIGs). This architectural dependency introduces significant operational penalties:
1. **Slow Provisioning Latency:** An ASG scale-up takes between **4 to 8 minutes** to detect pending pods, invoke cloud scaling APIs, boot instances, run bootstrap scripts, and join the cluster.
2. **Configuration Sprawl and Management Overhead:** If an engineering organization requires different instance families (e.g., AMD64 general-purpose, ARM64 Graviton, GPU acceleration, and Spot instances), administrators must manually define and manage dozens of distinct cloud ASGs.
3. **Inefficient Bin-Packing and Cost Waste:** Traditional Cluster Autoscaler cannot dynamically choose an arbitrary instance type from the cloud catalog; it can only adjust the desired replica count of pre-existing ASGs. If a pod requires 32GB of RAM and the active ASG consists of 8GB instances, the pod remains unschedulable unless an administrator provisions a new ASG.

Karpenter completely eliminates Auto Scaling Groups by adopting a **"group-less" direct-provisioning model**. Karpenter communicates directly with cloud compute APIs (such as AWS EC2 Fleet / `RunInstances`), spinning up perfectly right-sized virtual machines in **30 to 45 seconds**.

```text
+---------------------------------------------------------------------------------------------------+
|                        CLUSTER AUTOSCALER (ASG) VS KARPENTER (DIRECT API)                         |
+---------------------------------------------------------------------------------------------------+

 TRADITIONAL CLUSTER AUTOSCALER:
 [ Pending Pods ] ──► [ CA Controller ] ──► [ AWS Auto Scaling Group ] ──► [ EC2 API ]
 (Takes 4 - 8 Minutes! Bound to fixed, pre-configured instance types in the ASG)

 KARPENTER JUST-IN-TIME AUTOSCALING:
 [ Pending Pods ] ──► [ Karpenter Controller ] ─────────────────────────► [ EC2 RunInstances API ]
 (Takes 30 - 45 Seconds! Directly launches optimal instance type from entire 600+ EC2 catalog)
```

---

## 2. Core Karpenter Architectural Mechanisms

### 1. Just-in-Time Bin-Packing and Right-Sizing
When a batch of pods transitions to `Pending`, Karpenter analyzes the collective resource requirements across the entire batch (CPU requests, memory requests, GPU requirements, topology spread constraints, and node affinity). Karpenter's pricing engine then queries the real-time cloud pricing API and selects the single cheapest instance type capable of hosting the workload. For instance, instead of launching four separate `t3.xlarge` instances, Karpenter may launch a single high-efficiency `c6g.2xlarge` Graviton instance.

### 2. Automatic Node Consolidation and Defragmentation
Karpenter continuously watches cluster utilization through its **Consolidation engine**. If a worker node is running at low capacity and its pods can fit onto other active nodes, Karpenter automatically drains and terminates the node. If multiple smaller workloads are scattered across expensive machines, Karpenter can execute a "replacement consolidation": provisioning a single smaller, cheaper instance, migrating the pods, and destroying the expensive instances.

### 3. Native Spot Interruption Handling
When running on cost-effective Spot instances, Karpenter subscribes to an Amazon SQS queue that receives Amazon EventBridge events for:
- Spot Interruption Notices (sent 2 minutes before termination)
- Rebalance Recommendations (sent 15 minutes before termination)
- EC2 Instance State-change events
When a notice arrives, Karpenter immediately provisions a replacement instance and drains the terminating node before AWS physically reclaims the machine, ensuring zero downtime for stateful and stateless workloads.

---

## 3. The Karpenter Custom Resource Definitions (CRDs)

In modern Karpenter (`v1beta1` / `v1`), node provisioning is governed by two complementary Custom Resources:
1. **`NodePool`:** Defines workload constraints, resource limits, instance requirements (architecture, capacity type), and disruption rules.
2. **`EC2NodeClass`:** Defines AWS-specific infrastructure parameters such as AMI families, subnet selection, security groups, IAM roles, and storage block devices.

---

## 4. Production Manifests: NodePool and EC2NodeClass

```yaml
# 1. AWS-Specific Infrastructure Configuration
apiVersion: karpenter.k8s.aws/v1beta1
kind: EC2NodeClass
metadata:
  name: default-infrastructure
spec:
  amiFamily: AL2023
  role: "KarpenterNodeRole-my-cluster"
  subnetSelectorTerms:
    - tags:
        karpenter.sh/discovery: "my-cluster"
  securityGroupSelectorTerms:
    - tags:
        karpenter.sh/discovery: "my-cluster"
  blockDeviceMappings:
    - deviceName: /dev/xvda
      ebs:
        volumeSize: 50Gi
        volumeType: gp3
        iops: 3000
        throughput: 125
        encrypted: true
---
# 2. Kubernetes-Native Workload Constraints
apiVersion: karpenter.sh/v1beta1
kind: NodePool
metadata:
  name: general-compute-pool
spec:
  template:
    spec:
      nodeClassRef:
        apiVersion: karpenter.k8s.aws/v1beta1
        kind: EC2NodeClass
        name: default-infrastructure
      requirements:
        # Flexible architecture: support both Intel/AMD and ARM64 Graviton
        - key: kubernetes.io/arch
          operator: In
          values: ["amd64", "arm64"]
        # Allow both Spot and On-Demand capacity
        - key: karpenter.sh/capacity-type
          operator: In
          values: ["spot", "on-demand"]
        # Restrict instance categories: General purpose & Compute optimized
        - key: ec2.amazonaws.com/instance-category
          operator: In
          values: ["c", "m", "r"]
        # Filter out older generation instance families
        - key: ec2.amazonaws.com/instance-generation
          operator: Gt
          values: ["4"]
  # Consolidation and lifecycle settings
  disruption:
    consolidationPolicy: WhenUnderutilized
    expireAfter: 720h # Automatically recycle nodes every 30 days for patching
  # Upper limit to prevent runaway cloud bills
  limits:
    cpu: "1000"
    memory: 2000Gi
```

---

## 5. Architectural Comparison: Karpenter vs Cluster Autoscaler

| Architectural Dimension | Cluster Autoscaler (CA) | Karpenter |
| :--- | :--- | :--- |
| **Cloud Primitives** | Auto Scaling Groups (ASGs) / MIGs | Direct Cloud APIs (`RunInstances`) |
| **Scale-Up Latency** | 4 to 8 minutes | **30 to 45 seconds** |
| **Instance Type Selection** | Fixed to pre-defined ASG templates | **Dynamic right-sizing** across 600+ instance types |
| **ARM / Graviton Adoption** | Requires dedicated ARM ASGs | Dynamic bin-packing via `values: ["amd64", "arm64"]` |
| **Node Consolidation** | Removes empty nodes (utilization < 50%) | **Proactive defragmentation** and cost optimization |
| **Spot Disruption** | Relies on third-party termination handlers | **Native SQS / EventBridge** queue integration |
| **Configuration Overhead** | High (dozens of ASGs + Terraform code) | Low (single declarative `NodePool` CRD) |

---

## 6. Operational Debugging Commands

```bash
# Check active NodePool status and current resource usage against limits
kubectl get nodepool
kubectl describe nodepool general-compute-pool

# Inspect real-time provisioning, bin-packing, and consolidation decisions
kubectl logs -n kube-system -l app.kubernetes.io/name=karpenter -c controller --tail=100 -f

# Verify dynamically launched EC2 nodes and their capacity types
kubectl get nodes -L karpenter.sh/capacity-type,node.kubernetes.io/instance-type,kubernetes.io/arch
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Cluster Autoscaler Architecture and Cloud Integration](./04-Cluster-Autoscaler-Architecture-and-Cloud-Integration.md) | [Index](../../../README.md) | [06 - Autoscaling Anti Patterns Thrashing and Conflict Resolution →](./06-Autoscaling-Anti-Patterns-Thrashing-and-Conflict-Resolution.md) |
