# 05 - Services and Service Discovery

> **Learning Methodology:**
> **Understand → See → Practice → Troubleshoot → Interview → Revise**

## 📌 Module Syllabus
1. `01-Kubernetes-Service-Abstraction-and-Virtual-IPs.md` — Stable network identity: ClusterIP, virtual IP allocation, and label selector binding.
2. `02-Service-Types-ClusterIP-NodePort-and-LoadBalancer.md` — In-cluster vs node-level vs cloud-provider external traffic ingress.
3. `03-Headless-Services-and-Stateful-DNS-Discovery.md` — `clusterIP: None`: direct pod-to-pod communication and SRV records for stateful clusters.
4. `04-CoreDNS-Architecture-and-Name-Resolution-Flow.md` — DNS resolution in Kubernetes: `resolv.conf`, `ndots:5` search path penalty, and Corefile caching.
5. `05-Kube-Proxy-Modes-iptables-vs-IPVS-vs-Kernel-Routing.md` — Packet routing architectures: Netfilter chains vs Linux IPVS virtual servers vs eBPF bypass.
6. `06-EndpointSlices-High-Scale-Service-Endpoints.md` — Scalability beyond 1,000 endpoints: EndpointSlice controllers and chunked topology distribution.
7. `07-Real-World-Scenarios.md` — Production post-mortems: the CoreDNS 5-second lookup delay catastrophe, and NodePort port exhaustion.
8. `08-Troubleshooting.md` — Diagnostic runbooks for DNS timeouts, missing service endpoints, and kube-proxy desynchronization.
9. `09-Interview-QA.md` — 10 Senior SRE/DevOps interview scenarios on Kubernetes networking and Services.
10. `10-Hands-On-Practice.md` — End-to-end service testing: ClusterIP routing, CoreDNS queries via `dig`, and headless service resolution.
11. `11-MCQ.md` — 10 scenario-based multiple choice questions with detailed explanations.
12. `12-Quick-Revision.md` — High-density Service and DNS cheat sheet.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 04 - Deployments](../04-Deployments-ReplicaSets-and-Rollouts/README.md) | [README](./README.md) | [01 - Service Abstraction](./01-Kubernetes-Service-Abstraction-and-Virtual-IPs.md) |
