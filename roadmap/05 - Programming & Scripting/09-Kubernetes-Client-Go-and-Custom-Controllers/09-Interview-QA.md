# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the architectural difference between Edge-Triggered and Level-Triggered reconciliation?
**Answer**: Edge-triggered systems react to point-in-time state transitions (events). If an event is dropped or the receiver is down, state remains inconsistent. Level-triggered systems evaluate the **current actual state** against the **desired spec** on every cycle, guaranteeing eventual convergence regardless of past network partitions or restarts.

### Q2: Why does `client-go` use Informers and Listers instead of direct API queries?
**Answer**: Direct queries flood the `kube-apiserver` and `etcd` with high-frequency network and serialization overhead. An Informer maintains a single HTTP streaming watch connection, updates an in-memory cache (`Indexer`), and serves read queries locally from RAM via the `Lister` with zero network overhead.

### Q3: What is the function of a Kubernetes Finalizer?
**Answer**: A finalizer is a pre-delete hook stored in `metadata.finalizers`. When an object is deleted, Kubernetes sets `deletionTimestamp` but does not purge the object from `etcd` until the controller performs asynchronous cleanup (e.g., deleting cloud databases) and removes its finalizer string from the array.

### Q4: Explain the role of `predicate.Funcs` in `controller-runtime`.
**Answer**: Predicates filter out irrelevant watch events before they enter the controller's Workqueue. For example, `GenerationChangedPredicate` prevents status updates or resync timers from triggering redundant reconciliation loops if the object's `spec` has not changed.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
