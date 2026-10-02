# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Controller Hot-Loop Denial-of-Service on `kube-apiserver`

### Context & Incident
A platform engineering team deployed a custom Operator to reconcile network security policies across 10,000 developer namespaces. Shortly after deployment, all control plane operations crashed, `kubectl` timed out with HTTP 504, and etcd CPU spiked to 100%.

### Root Cause
Inside the controller's `Reconcile()` loop:
```go
// FATAL FLAW: Updating object triggers a new Update event on the Informer!
networkPolicy.Annotations["last-reconciled"] = time.Now().String()
r.Update(ctx, &networkPolicy)
```
Each time the controller updated the timestamp annotation, the API server persisted the change. The Informer detected the update and pushed the key back onto the Workqueue! This triggered an **infinite, tight recursive hot-loop** across 10,000 resources, flooding the API server with 50,000 write requests per second.

### Architectural Prevention
1. Use **Predicate Filters** (`predicate.GenerationChangedPredicate`) to ignore metadata/annotation changes that do not alter the `spec.generation`.
2. Store operational timestamps in the **Status** subresource rather than metadata annotations, and configure informers to ignore status-only updates.

---

## Scenario 2: The Infinite Namespace Deletion Deadlock (Stuck Finalizers)

### Context & Incident
A developer deleted a test namespace (`kubectl delete ns staging`). The namespace remained stuck indefinitely in the `Terminating` state, preventing cluster cleanups and CI runner resets.

### Root Cause
A custom CRD instance had a finalizer (`database.example.com/finalizer`). The Operator responsible for executing the cleanup had crashed and was removed from the cluster earlier in the week. The Kubernetes garbage collector cannot delete an object with active finalizers!

### Remediation & Runbook
```bash
# 1. Identify resources blocking namespace deletion
kubectl api-resources --verbs=list --namespaced -o name | \
  xargs -n 1 kubectl get --show-kind --ignore-not-found -n staging

# 2. Patch out the orphaned finalizer to release the deadlock
kubectl patch dbi my-db -n staging --type=merge -p '{"metadata":{"finalizers":[]}}'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Building CLIs with Cobra and Viper](./06-Building-Enterprise-CLIs-with-Cobra-and-Viper.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
