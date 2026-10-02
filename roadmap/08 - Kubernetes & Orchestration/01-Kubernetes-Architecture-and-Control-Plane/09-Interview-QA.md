# 09 - Interview Questions & Architectural Scenarios

### Q1: What happens under the hood when you execute `kubectl run nginx --image=nginx`?
**Answer:**
1. `kubectl` parses the command, builds an HTTP POST request containing the PodSpec, and submits it to `kube-apiserver`.
2. `kube-apiserver` runs Authentication (AuthN), Authorization (AuthZ), Mutating Admission Webhooks, Schema Validation, and Validating Admission Webhooks.
3. The Pod object is written to `etcd` in the `Pending` state with no `nodeName`.
4. `kube-scheduler` detects the unassigned Pod via its watch stream, filters eligible worker nodes (Predicates), ranks them (Priorities), and submits a `Binding` object back to the API server assigning `nodeName`.
5. The `kubelet` on the selected worker node detects the scheduled Pod via its watch stream.
6. The `kubelet` instructs the CRI runtime (`containerd`) via gRPC to pull the image, invokes CNI to allocate an IP and veth interface, and invokes `runc` to create the container.
7. `kubelet` reports `Running` state back to `kube-apiserver`, which updates `etcd`.

---

### Q2: Why is etcd cluster size always an odd number (3, 5, 7)?
**Answer:**
etcd uses the Raft consensus algorithm, which requires a strict majority quorum ($Q = \lfloor N/2 floor + 1$) to commit any transaction. An odd number of nodes provides the optimal balance of fault tolerance without added network overhead. For example, a 4-node cluster requires 3 nodes for quorum (tolerating 1 failure). A 3-node cluster also requires 2 nodes for quorum (tolerating 1 failure). Adding the 4th node increases network latency and consensus overhead without providing any additional fault tolerance.

---

### Q3: How does kubelet know whether to restart a failed container?
**Answer:**
The `kubelet` evaluates the Pod's `restartPolicy` (`Always`, `OnFailure`, `Never`). It applies an exponential backoff delay (`10s`, `20s`, `40s`, up to `300s`) between restarts, which manifests as the `CrashLoopBackOff` status in `kubectl get pods`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
