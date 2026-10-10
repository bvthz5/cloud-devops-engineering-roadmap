# 02 - Rolling Update Strategy: MaxSurge and MaxUnavailable

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to zero-downtime rolling updates in Kubernetes, detailing mathematical calculations for `maxSurge` and `maxUnavailable`, trade-off profiles, and CLI rollout controls.

---

## 🏛️ 1. What Is a Rolling Update & Why Avoid Recreate?

When shipping new application versions to a production environment, engineering teams face a critical choice between two fundamental deployment strategies supported by Kubernetes:

1. **`Recreate` Strategy (Downtime Deployment):**
   - The Deployment Controller terminates and deletes **every single existing Pod simultaneously**.
   - Only after all old Pods have fully terminated does the controller begin scheduling the new version.
   - **The Consequence:** Your service experiences **100% downtime** for several minutes while the old pods shut down and the new pods download images and boot. The `Recreate` strategy is suitable only for development environments or legacy stateful applications that cannot tolerate two concurrent versions accessing the same database schema.
2. **`RollingUpdate` Strategy (Zero-Downtime Deployment):**
   - The Deployment Controller incrementally replaces old Pod instances with new Pod instances in a controlled, phased sequence.
   - New pods are created, tested via readiness probes, and added to the Service routing mesh before corresponding old pods are decommissioned.
   - **The Result:** The application services continuous end-user HTTP traffic with **zero downtime** throughout the entire deployment process.

---

## 📐 2. The Mathematics of `maxSurge` and `maxUnavailable`

The pace, safety, and infrastructure footprint of a rolling update are governed entirely by two parameters declared under `spec.strategy.rollingUpdate`: **`maxSurge`** and **`maxUnavailable`**.

Both parameters can be declared either as an absolute integer (e.g., `2`) or as a percentage of the desired replica count (e.g., `25%`):

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   ROLLING UPDATE MATHEMATICAL BOUNDARIES               │
└────────────────────────────────────────────────────────────────────────┘

 Desired Replicas: R = 8
 maxSurge: 25% (25% of 8 = 2)
 maxUnavailable: 25% (25% of 8 = 2)
 
 1. Maximum Pod Ceiling Allowed During Update:
    Max Total Pods = Desired Replicas + maxSurge = 8 + 2 = 10 Pods
 
 2. Minimum Healthy Floor Required During Update:
    Min Available Pods = Desired Replicas - maxUnavailable = 8 - 2 = 6 Pods
```

### Detailed Parameter Definitions:
- **`maxSurge` (The Buffer Ceiling):** Specifies the maximum number of Pods that Kubernetes is allowed to create **above and beyond** the desired replica count during an update. (Percentages are rounded *up* to the nearest integer). A higher `maxSurge` accelerates rollouts but requires spare CPU and memory capacity on the worker nodes.
- **`maxUnavailable` (The Tolerance Floor):** Specifies the maximum number of Pods that are permitted to be in an unavailable (terminating or non-ready) state during the update process relative to the desired replica count. (Percentages are rounded *down* to the nearest integer).
- **⚠️ The Deadlock Rule:** You cannot set both `maxSurge: 0` AND `maxUnavailable: 0` simultaneously. Doing so would command Kubernetes to create zero new pods and terminate zero old pods, permanently freezing the rollout in a mathematical deadlock!

---

## 📊 3. Step-by-Step Rollout Progression Walkthrough

Consider a microservice configured with `replicas: 4`, `maxSurge: 1` (25%), and `maxUnavailable: 0`:

```text
┌────────────────────────────────────────────────────────────────────────┐
│               ZERO-DOWNTIME STEP-BY-STEP PROGRESSION                   │
│               (Replicas: 4, maxSurge: 1, maxUnavailable: 0)            │
└────────────────────────────────────────────────────────────────────────┘

 Step 0: Baseline State
 ├── Old ReplicaSet (v1): [ Pod 1, Pod 2, Pod 3, Pod 4 ] (4 Healthy)
 └── New ReplicaSet (v2): [ ] (0 Pods)
 Total Active: 4 / Minimum Floor: 4 / Maximum Ceiling: 5

 Step 1: Surge New Version
 ├── Old ReplicaSet (v1): [ Pod 1, Pod 2, Pod 3, Pod 4 ] (4 Healthy)
 └── New ReplicaSet (v2): [ Pod 5 (Starting...) ] (1 Pending)
 Total Active: 5 (MaxSurge Ceiling Reached!)

 Step 2: Readiness Verification & Initial Scale Down
 ├── Pod 5 passes readiness probe! Now marked Healthy!
 ├── Kubelet safely terminates Pod 4 from Old ReplicaSet!
 ├── Old ReplicaSet (v1): [ Pod 1, Pod 2, Pod 3 ] (3 Healthy)
 └── New ReplicaSet (v2): [ Pod 5 ] (1 Healthy)
 Total Healthy: 4 (Minimum Floor Maintained!)

 Step 3: Progressive Replacement Continues
 ├── New ReplicaSet launches Pod 6 ──► Becomes Healthy ──► Terminates Pod 3
 ├── New ReplicaSet launches Pod 7 ──► Becomes Healthy ──► Terminates Pod 2
 ├── New ReplicaSet launches Pod 8 ──► Becomes Healthy ──► Terminates Pod 1

 Step 4: Rollout Completed
 ├── Old ReplicaSet (v1): [ ] (0 Replicas)
 └── New ReplicaSet (v2): [ Pod 5, Pod 6, Pod 7, Pod 8 ] (4 Healthy)
 100% of user traffic transitioned with ZERO dropped packets!
```

---

## 🎯 4. Production Strategy Profiles Matrix

Depending on business criticality and infrastructure budgets, DevOps teams select from four primary rolling update configuration profiles:

| Strategy Profile | `maxSurge` | `maxUnavailable` | Operational Behavior & Architectural Trade-Off |
| :--- | :---: | :---: | :--- |
| **Mission-Critical (Strict SLA)** | `25%` | `0` | **Zero Capacity Reduction.** Available capacity never drops below 100%. Requires spare compute capacity across worker nodes to handle the surge. |
| **Balanced Enterprise (Default)** | `25%` | `25%` | **Standard Cloud Balance.** Rapid rollout speed with balanced resource overhead. Capacity can temporarily dip by up to 25%. |
| **Cost-Constrained (Edge / Bare-Metal)** | `0` | `1` | **Zero Extra Compute Footprint.** Does not require spare node memory or CPU. Terminates one old pod before launching each new pod. |
| **Instantaneous Surge** | `100%` | `0` | **Fastest Rollout.** Doubles the entire workload instantly (launches 10 new pods alongside 10 old pods), cutting over immediately once ready. |

---

## 🛠️ 5. Practical CLI Rollout Management

```bash
# 1. Update deployment image to trigger a rolling update
kubectl set image deployment/web-api api=mycompany/web-api:v2.2.0

# 2. Watch real-time rollout progression and pod readiness
kubectl rollout status deployment/web-api

# 3. Pause an in-flight rollout (useful for manual canary inspection)
kubectl rollout pause deployment/web-api

# 4. Resume a paused rollout to continue updating remaining pods
kubectl rollout resume deployment/web-api

# 5. Instantly abort and roll back to previous stable ReplicaSet
kubectl rollout undo deployment/web-api
```

---

## 📋 6. Summary and Navigation

Mastering the mathematical mechanics of `maxSurge` and `maxUnavailable` allows engineering teams to deploy microservices continuously without impacting active end users. By pairing strict zero-unavailable floors with robust readiness probes, Kubernetes provides an enterprise deployment engine capable of shipping thousands of daily releases with total reliability.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Deployment Controller and ReplicaSet Reconciliation](./01-Deployment-Controller-and-ReplicaSet-Reconciliation.md) | [Index](../../../README.md) | [03 - Rollbacks Revision History and Change Cause →](./03-Rollbacks-Revision-History-and-Change-Cause.md) |
