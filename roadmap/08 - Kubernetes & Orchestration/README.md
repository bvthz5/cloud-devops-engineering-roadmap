# 08 - Kubernetes & Orchestration

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

Kubernetes is the de facto cloud operating system and container orchestration standard for enterprise microservices, platform engineering, and high-availability distributed systems. A Senior DevOps/Cloud Engineer, Platform Engineer, or SRE must master Kubernetes from raw control plane mechanics (kube-apiserver, etcd Raft consensus, kube-scheduler, kubelet) to advanced workloads (StatefulSets, DaemonSets, Jobs), zero-downtime rolling updates, virtual networking (CNI, CoreDNS, Cilium eBPF, NetworkPolicies), storage (CSI, StorageClasses, VolumeSnapshots), package management (Helm v3), autoscaling (HPA v2, VPA, Karpenter), custom extensions (CRDs and Go Operators), the next-generation Gateway API, and enterprise disaster recovery (etcd snapshotting and Velero).

---

## 📌 Complete Curriculum Modules

| # | Module | Core Focus & Engineering Scope | Status |
|---|---|---|:---:|
| 01 | [**01. Kubernetes Architecture & Control Plane**](./01-Kubernetes-Architecture-and-Control-Plane/README.md) | Control plane components (API server, etcd Raft, scheduler, controller manager), worker plane (kubelet PLEG, kube-proxy, CRI), API request pipeline, and HA topologies. | ✅ Complete |
| 02 | [**02. Kubectl & Local Clusters (Kind / Minikube)**](./02-Kubectl-and-Local-Clusters/README.md) | Kubectl architecture, `~/.kube/config` contexts, JSONPath/custom-columns, Krew plugins (`ctx`, `ns`, `neat`), multi-node Kind topologies, K3s, and Kubeadm bootstrap. | ✅ Complete |
| 03 | [**03. Pods & Workloads**](./03-Pods-and-Workloads/README.md) | Pod atomic architecture, pause container, lifecycle phases, health probes (Startup, Readiness, Liveness), native sidecars, QoS classes, and SecurityContexts. | ✅ Complete |
| 04 | [**04. Deployments, ReplicaSets & Rollouts**](./04-Deployments-ReplicaSets-and-Rollouts/README.md) | ReplicaSet reconciliation, zero-downtime rolling updates (`maxSurge`, `maxUnavailable`), rollbacks, revision history, native Canary/Blue-Green, and `preStop` hooks. | ✅ Complete |
| 05 | [**05. Services & Service Discovery**](./05-Services-and-Service-Discovery/README.md) | Service virtual IPs, ClusterIP, NodePort, LoadBalancer, Headless Services (`clusterIP: None`), CoreDNS architecture (`ndots:5`), kube-proxy modes, and EndpointSlices. | ✅ Complete |
| 06 | [**06. Ingress Controllers & Routing**](./06-Ingress-Controllers-and-Routing/README.md) | Layer 7 routing, Ingress-Nginx controller architecture, Lua shared-memory dynamic routing, cert-manager automated TLS, canary annotations, and ModSecurity WAF. | ✅ Complete |
| 07 | [**07. ConfigMaps & Secrets**](./07-ConfigMaps-and-Secrets/README.md) | Decoupling configuration from pods, Base64 encoding realities, etcd KMS envelope encryption at rest, `immutable: true` scaling, External Secrets Operator (ESO), and Reloader. | ✅ Complete |
| 08 | [**08. Storage: PV, PVC & StorageClasses**](./08-Storage-PV-PVC-and-StorageClasses/README.md) | Persistent storage decoupling, StorageClasses, `volumeBindingMode: WaitForFirstConsumer`, CSI drivers, access modes (RWO, RWX, RWOP), volume expansion, and VolumeSnapshots. | ✅ Complete |
| 09 | [**09. StatefulSets & DaemonSets**](./09-StatefulSets-and-DaemonSets/README.md) | Stateful workloads, stable ordinals, `volumeClaimTemplates` retention, `OrderedReady` vs `Parallel`, clustered database patterns, and DaemonSet node agents. | ✅ Complete |
| 10 | [**10. Jobs & CronJobs**](./10-Jobs-and-CronJobs/README.md) | Run-to-completion batch workloads, work queues, `backoffLimit`, `podFailurePolicy`, CronJob 5-part syntax, timezones, concurrency policies (`Forbid`), and `ttlSecondsAfterFinished`. | ✅ Complete |
| 11 | [**11. Auto-Scaling: HPA, VPA & Cluster Autoscaler**](./11-Auto-Scaling-HPA-VPA-Cluster-Autoscaler/README.md) | HPA v2 target formulas, custom metrics via Prometheus Adapter, VPA recommender/updater, Cluster Autoscaler pending simulation, Karpenter just-in-time nodes, and flapping defense. | ✅ Complete |
| 12 | [**12. RBAC & Cluster Security**](./12-RBAC-and-Cluster-Security/README.md) | Authentication (X.509, OIDC), Role, ClusterRole, RoleBinding, Bound ServiceAccount token projection, Pod Security Admission (PSA), admission webhooks, and Kyverno / OPA policies. | ✅ Complete |
| 13 | [**13. Helm Package Manager & Charts**](./13-Helm-Package-Manager-and-Charts/README.md) | Tillerless Helm v3 architecture, Secrets release state, chart directory layout, Go templating & Sprig pipelines, subcharts, lifecycle hooks, and OCI chart registries. | ✅ Complete |
| 14 | [**14. Kubernetes Networking, CNI & NetworkPolicies**](./14-Kubernetes-Networking-CNI-and-NetworkPolicies/README.md) | IP-per-Pod model, CNI specification, Calico Layer 3 BGP routing, Cilium eBPF datapath & Hubble, Default Deny Zero-Trust NetworkPolicies, and L7 FQDN egress filtering. | ✅ Complete |
| 15 | [**15. CRDs & Kubernetes Operators**](./15-CRDs-and-Kubernetes-Operators/README.md) | CustomResourceDefinitions, OpenAPI v3 validation, Operator pattern, level-triggered reconciliation loops, Kubebuilder Go scaffolding, finalizers, and Operator Lifecycle Manager (OLM). | ✅ Complete |
| 16 | [**16. Kubernetes Troubleshooting & Debugging**](./16-Kubernetes-Troubleshooting-and-Debugging/README.md) | CrashLoopBackOff, OOMKilled, ImagePullBackOff, Node NotReady forensics, PLEG diagnosis, network packet loss, etcd loss of quorum, and ephemeral debug containers (`kubectl debug`). | ✅ Complete |
| 17 | [**17. Advanced Scheduling, Taints, Tolerations & Affinity**](./17-Advanced-Scheduling-Taints-Tolerations-and-Affinity/README.md) | Scheduler framework (filtering & scoring), NodeAffinity (hard vs soft), PodAffinity/Anti-Affinity, Taints and Tolerations, Topology Spread Constraints, and Descheduler. | ✅ Complete |
| 18 | [**18. Gateway API & Modern Traffic Routing**](./18-Gateway-API-and-Modern-Traffic-Routing/README.md) | Official successor to Ingress: role-oriented architecture (`GatewayClass`, `Gateway`, `HTTPRoute`), native URL rewrites, canary weighting, mirroring, and `ReferenceGrant` security. | ✅ Complete |
| 19 | [**19. Cluster Backup, Disaster Recovery & Velero**](./19-Cluster-Backup-Disaster-Recovery-and-Velero/README.md) | Enterprise DR strategy (RTO/RPO), `etcdctl snapshot save/restore`, Velero backup server, CSI VolumeSnapshot orchestration, scheduled backups to S3, and disaster testing. | ✅ Complete |

---

## 🛠️ Module Structure Standard

Every module in this domain strictly adheres to the enterprise 13-file roadmap standard:
1. `README.md` — Architectural overview & module syllabus
2. `01-` to `06-` — Deep technical guides with diagrams, code blocks, and internal mechanisms
3. `07-Real-World-Scenarios.md` — Real production post-mortems and multi-stage failure analyses
4. `08-Troubleshooting.md` — Diagnostic decision trees and debugging runbooks
5. `09-Interview-QA.md` — 10 Senior SRE/DevOps technical interview scenarios
6. `10-Hands-On-Practice.md` — Hands-on implementation labs with production blueprints
7. `11-MCQ.md` — 10 scenario-based multiple choice questions with collapsible explanations
8. `12-Quick-Revision.md` — High-density cheat sheets and command reference tables

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Containers & Docker](../07%20-%20Containers%20&%20Docker/README.md) | [Master Index](../00-Master-Index.md) | [09 - Infrastructure as Code (Terraform & OpenTofu)](../09%20-%20Infrastructure%20as%20Code%20(Terraform%20&%20OpenTofu)/README.md) |
