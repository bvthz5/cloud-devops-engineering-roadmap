# 01 - Horizontal Pod Autoscaler (HPA v2) and Metrics APIs

## 1. What Is Horizontal Pod Autoscaling?

The **Horizontal Pod Autoscaler (HPA)** is a Kubernetes control plane controller that dynamically adjusts the number of replica Pods in a workload (Deployment, ReplicaSet, or StatefulSet) to match observed computational demand.

In cloud computing, scaling workloads takes two primary forms:
1. **Vertical Scaling (Scaling Up):** Increasing the CPU or RAM allocations of an existing virtual machine or container. This requires terminating or restarting the container, introducing downtime.
2. **Horizontal Scaling (Scaling Out):** Adding or removing identical, parallel Pod replicas behind a load-balancing Service. This achieves elasticity and zero downtime.

HPA operates at the horizontal layer. It continuously polls metrics APIs, calculates the mathematically ideal number of replicas required to maintain target utilization, and modifies the `spec.replicas` field of the target workload object.

```text
+---------------------------------------------------------------------------------------------------+
|                        THE KUBERNETES METRICS PIPELINE AND HPA CONTROL LOOP                       |
+---------------------------------------------------------------------------------------------------+

 ┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ CONTROL PLANE: Kube-Controller-Manager                                                          │
 │                                                                                                 │
 │  ┌───────────────────────────────────────────────────────────────────────────────────────────┐  │
 │  │ HPA Controller Loop (Executes every 15 seconds: --horizontal-pod-autoscaler-sync-period)   │  │
 │  │ 1. Queries Metrics API for current utilization                                            │  │
 │  │ 2. Calculates: DesiredReplicas = ceil[ CurrentReplicas * (CurrentMetric / TargetMetric) ]  │  │
 │  │ 3. Applies stabilization window to prevent flapping                                       │  │
 │  │ 4. Updates Deployment: spec.replicas = DesiredReplicas                                    │  │
 │  └───────────────────────────────┬───────────────────────────────────────────────────────────┘  │
 └──────────────────────────────────┼──────────────────────────────────────────────────────────────┘
                                    │ Queries via Aggregated API
                                    ▼
 ┌─────────────────────────────────────────────────────────────────────────────────────────────────┐
 │ METRICS AGGREGATION LAYER: metrics-server                                                       │
 │ Exposes REST Endpoint: /apis/metrics.k8s.io/v1beta1/pods                                        │
 └──────────────────────────────────┬──────────────────────────────────────────────────────────────┘
                                    │ Scrapes periodically via kubelet summary API (:10250)
         ┌──────────────────────────┴──────────────────────────┐
         ▼                                                     ▼
 ┌───────────────────────────┐                         ┌───────────────────────────┐
 │ WORKER NODE 01            │                         │ WORKER NODE 02            │
 │  ┌─────────────────────┐  │                         │  ┌─────────────────────┐  │
 │  │ Kubelet (cAdvisor)  │  │                         │  │ Kubelet (cAdvisor)  │  │
 │  │ Reads cgroup CPU/RAM│  │                         │  │ Reads cgroup CPU/RAM│  │
 │  └──────────┬──────────┘  │                         │  └──────────┬──────────┘  │
 │             ▼             │                         │             ▼             │
 │    [Pod A]    [Pod B]     │                         │    [Pod C]    [Pod D]     │
 └───────────────────────────┘                         └───────────────────────────┘
```

---

## 2. The 3 Kubernetes Metrics API Pipelines

Kubernetes divides autoscaling telemetry into three distinct API pipelines:

```text
+---------------------------------------------------------------------------------------------------+
|                          THE THREE METRICS API ARCHITECTURES                                      |
+---------------------------------------------------------------------------------------------------+

 1. Resource Metrics API (metrics.k8s.io):
    Provider: metrics-server
    Source: Linux kernel cgroups via Kubelet cAdvisor.
    Metrics: CPU (millicores) and Working Set Memory (bytes).
    Use Case: Standard CPU-based autoscaling.

 2. Custom Metrics API (custom.metrics.k8s.io):
    Provider: Prometheus Adapter / Datadog Cluster Agent.
    Source: Internal application telemetry scraped from within the cluster.
    Metrics: HTTP requests per second, active database connection pool size, JVM heap usage.
    Use Case: Autoscaling based on real application traffic throughput.

 3. External Metrics API (external.metrics.k8s.io):
    Provider: KEDA (Kubernetes Event-driven Autoscaling) / Cloud Provider Adapters.
    Source: External cloud services outside the Kubernetes cluster.
    Metrics: AWS SQS queue depth, GCP Pub/Sub unacknowledged messages, Kafka lag.
    Use Case: Autoscaling asynchronous batch workers before CPU spikes occur.
```

---

## 3. The Core Mathematical Scaling Algorithm

The HPA controller determines desired replicas using the standard ceiling formula:

$$\text{DesiredReplicas} = \left\lceil \text{CurrentReplicas} \times \left( \frac{\text{CurrentMetricValue}}{\text{TargetMetricValue}} \right) \right\rceil$$

### Step-by-Step Numerical Example:
Suppose an online e-commerce deployment has:
- `CurrentReplicas` = $5$
- `TargetCPUUtilization` = $50\%$ (relative to Pod `resources.requests.cpu`)
- High traffic arrives, causing the average CPU consumption across all 5 pods to jump to $82\%$ (`CurrentMetricValue`).

$$\text{DesiredReplicas} = \left\lceil 5 \times \left( \frac{82}{50} \right) \right\rceil = \lceil 5 \times 1.64 \rceil = \lceil 8.2 \rceil = 9 \text{ Replicas}$$

The HPA controller immediately patches the Deployment from 5 to 9 replicas.

### The 10% Default Tolerance Buffer:
To prevent unnecessary pod churn caused by micro-fluctuations in CPU usage, the HPA controller incorporates a built-in **$10\%$ tolerance band**:
$$\text{If } 0.90 \le \frac{\text{CurrentMetricValue}}{\text{TargetMetricValue}} \le 1.10 \implies \text{No Scaling Action Taken!}$$
If target utilization is 50% and current utilization is 53%, the ratio is $1.06$, which falls inside the tolerance band. HPA suppresses the scale event.

---

## 4. Production Manifest: HPA v2 with Advanced Stabilization Behavior

In high-throughput enterprise environments, naive autoscaling can cause **flapping (thrashing)**: a traffic spike triggers rapid scale-up, followed by an immediate drop in per-pod utilization that causes rapid scale-down, followed immediately by another spike. 

The `autoscaling/v2` specification allows granular control over scale-up and scale-down velocity via `behavior`:

```yaml
apiVersion: autoscaling/v2
kind: HorizontalPodAutoscaler
metadata:
  name: payment-service-hpa
  namespace: finance
spec:
  scaleTargetRef:
    apiVersion: apps/v1
    kind: Deployment
    name: payment-service
  minReplicas: 4
  maxReplicas: 40
  metrics:
    # 1. Scale on CPU utilization (percentage of request)
    - type: Resource
      resource:
        name: cpu
        target:
          type: Utilization
          averageUtilization: 65
    # 2. Scale on Memory working set size
    - type: Resource
      resource:
        name: memory
        target:
          type: AverageValue
          averageValue: 500Mi
  behavior:
    # Aggressive Scale-Up: Respond to sudden traffic surges instantly
    scaleUp:
      stabilizationWindowSeconds: 0
      policies:
        - type: Percent
          value: 100        # Double the number of replicas immediately
          periodSeconds: 15
        - type: Pods
          value: 4          # Or add 4 pods every 15s (whichever is higher)
          periodSeconds: 15
      selectPolicy: Max
    # Conservative Scale-Down: Drain slowly to prevent thrashing
    scaleDown:
      stabilizationWindowSeconds: 300   # Look back 5 minutes; use the peak replica count
      policies:
        - type: Percent
          value: 10                     # Remove at most 10% of pods per minute
          periodSeconds: 60
      selectPolicy: Min
```

---

## 5. Critical Autoscaling Anti-Patterns to Avoid

### 1. The Missing Resource Request Trap
HPA **cannot compute CPU percentage utilization** if the target container manifest does not declare `resources.requests.cpu`. If requests are omitted, HPA reports `unknown/70%` and scaling is permanently disabled.

### 2. Autoscaling on Memory without Careful Consideration
Unlike CPU, which is compressible (Linux CFS throttles excess cycles), memory is non-compressible. Furthermore, runtimes with garbage collection (such as Java JVM, Go, Node.js) allocate memory and do not immediately return it to the operating system. Consequently, a memory-based HPA may scale up and never scale down because the runtime continues to hold allocated heap memory.

### 3. Concurrent HPA and VPA on CPU/Memory
Running both HPA and VPA (Vertical Pod Autoscaler) on the same resource metric creates catastrophic race conditions: HPA attempts to add more pods while VPA simultaneously attempts to enlarge the existing pods. Always decouple them: use HPA for CPU/RAM and VPA in `Off` (recommendation-only) mode.

---

## 6. Operational Debugging Commands

```bash
# Inspect current HPA target vs observed values
kubectl get hpa payment-service-hpa -n finance

# View detailed scaling history, calculation events, and metrics-server errors
kubectl describe hpa payment-service-hpa -n finance

# Directly inspect raw metrics emitted by the Metrics API
kubectl get --raw "/apis/metrics.k8s.io/v1beta1/pods" | jq .
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (10-Jobs-and-CronJobs)](../10-Jobs-and-CronJobs/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Custom and External Metrics with Prometheus Adapter →](./02-Custom-and-External-Metrics-with-Prometheus-Adapter.md) |
