# 12 - Quick-Revision & Enterprise Cheat Sheet

## Core Architectural Responsibilities

| Component | Execution Mode | Critical Role | Port |
|---|---|---|---|
| **kube-apiserver** | Static Pod / Binary | REST API, AuthN, AuthZ, Admission, etcd gatekeeper | `6443` |
| **etcd** | Static Pod / Dedicated | Distributed Raft key-value database | `2379`, `2380` |
| **kube-scheduler** | Static Pod | Node filtering (Predicates) & scoring (Priorities) | `10259` |
| **kube-controller-manager**| Static Pod | Reconciles actual state with desired state | `10257` |
| **kubelet** | Systemd Service | Node supervisor, PLEG sync loop, cgroups, probes | `10250` |
| **kube-proxy** | DaemonSet / Static Pod | ClusterIP routing via iptables / IPVS | `10256` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - MCQ](./11-MCQ.md) | [README](./README.md) | [Module 02 - Kubectl & Local Clusters](../02-Kubectl-and-Local-Clusters/README.md) |
