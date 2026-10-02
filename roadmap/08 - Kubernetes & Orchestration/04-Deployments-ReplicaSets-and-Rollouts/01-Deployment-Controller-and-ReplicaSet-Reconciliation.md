# 01 - Deployment Controller and ReplicaSet Reconciliation

## 1. The Deployment Hierarchy

A **Deployment** provides declarative updates for Pods and ReplicaSets. You describe a desired state in a Deployment, and the Deployment Controller changes the actual state to the desired state at a controlled rate.

```text
+-------------------------------------------------------------------------------+
|                                  DEPLOYMENT                                   |
|   spec.replicas: 3                                                            |
|   spec.strategy.type: RollingUpdate                                           |
+---------------------------------------+---------------------------------------+
                                        | Manages
                                        v
+-------------------------------------------------------------------------------+
|                            REPLICASET (Version 1)                             |
|   pod-template-hash: 7d648b9c8                                                |
|   replicas: 3                                                                 |
+-------------------+-------------------+-------------------+-------------------+
                    |                   |                   |
                    v                   v                   v
            +---------------+   +---------------+   +---------------+
            |  Pod (v1.0)   |   |  Pod (v1.0)   |   |  Pod (v1.0)   |
            +---------------+   +---------------+   +---------------+
```

---

## 2. Pod-Template-Hash Mechanics

When a Deployment is created or updated, the Deployment Controller computes a 32-bit FNV-1a hash of the `spec.template` (e.g., `7d648b9c8`).
- The controller appends this hash to the ReplicaSet name: `app-deployment-7d648b9c8`.
- The controller injects a label `pod-template-hash: 7d648b9c8` into both the ReplicaSet selector and the Pod labels.
- This ensures that child ReplicaSets and Pods do not collide across revisions.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Rolling Update Strategy](./02-Rolling-Update-Strategy-MaxSurge-and-MaxUnavailable.md) |
