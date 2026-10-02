# 01 - Kubernetes Architecture and Control Plane

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Control-Plane-Components-API-etcd-Scheduler-ControllerManager.md` — The brain of Kubernetes: kube-apiserver, etcd, kube-scheduler, and kube-controller-manager.
2. `02-Node-Components-Kubelet-KubeProxy-and-CRI.md` — The worker plane: kubelet, kube-proxy, Container Runtime Interface (CRI), and cgroup drivers.
3. `03-Etcd-Distributed-Storage-Quorum-and-Raft.md` — The single source of truth: Raft consensus, write paths, leader election, quorum calculations, and snapshotting.
4. `04-Kubernetes-API-Request-Flow-Authentication-Admission.md` — The lifecycle of a request: Transport security, Authentication (AuthN), Authorization (AuthZ), and Mutating/Validating Admission.
5. `05-High-Availability-Control-Plane-Topologies.md` — Stacked etcd vs external etcd, HAProxy/Keepalived load balancing, and split-brain defense.
6. `06-Cloud-Controller-Manager-and-Provider-Integrations.md` — Decoupling cloud provider APIs: node lifecycle, cloud routes, and Layer 4 load balancer provisioning.
7. `07-Real-World-Scenarios.md` — Production post-mortems: etcd disk latency crashes, kube-apiserver OOMs, and control plane partition recoveries.
8. `08-Troubleshooting.md` — Step-by-step diagnostic decision trees for control plane component failures.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Kubernetes architecture.
10. `10-Hands-On-Practice.md` — Production lab: manual inspection of static pods, etcdctl queries, and API request tracing.
11. `11-MCQ.md` — 10 scenario-based multiple-choice questions with deep explanations.
12. `12-Quick-Revision.md` — High-density architecture cheat sheet and operational reference.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Master Index](../../00-Master-Index.md) | [README](./README.md) | [01 - Control Plane Components](./01-Control-Plane-Components-API-etcd-Scheduler-ControllerManager.md) |
