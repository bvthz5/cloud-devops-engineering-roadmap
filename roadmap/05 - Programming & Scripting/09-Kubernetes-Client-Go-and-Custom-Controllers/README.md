# 09 - Kubernetes Client-Go and Custom Controllers

The modern cloud native ecosystem runs on Kubernetes. Beyond applying static YAML manifests, Platform Engineers and Site Reliability Engineers must build **automated controllers, operators, and custom CLIs** to extend the Kubernetes control plane. Built with **Go** and **`client-go`**, custom controllers reconcile desired state against current state in real-time, automating complex operational runbooks, custom resource lifecycles (CRDs), and self-healing infrastructure.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Kubernetes API Machinery & client-go Architecture](./01-Kubernetes-API-Machinery-and-client-go-Architecture.md) | API server communication, `RESTClient`, `Clientset`, `DynamicClient`, `clientcmd` auth. |
| 02 | [Informers, Listers & the Workqueue Pattern](./02-Informers-Listers-and-the-Workqueue-Pattern.md) | `SharedInformerFactory`, `Reflector`, `DeltaFIFO`, `Indexer` cache, rate-limiting `Workqueue`. |
| 03 | [Custom Resource Definitions (CRDs) & Code-Gen](./03-Custom-Resource-Definitions-CRDs-and-Code-Generation.md) | OpenAPI v3 schemas, `k8s.io/code-generator` tags, deepcopy, typed clientsets and listers. |
| 04 | [Writing a Controller: The Reconcile Loop](./04-Writing-a-Kubernetes-Controller-The-Reconcile-Loop.md) | Level-triggered state reconciliation, edge cases, error backoff, status updates, finalizers. |
| 05 | [Operator SDK & Kubebuilder Framework](./05-Operator-SDK-and-Kubebuilder-Framework.md) | Kubebuilder scaffolding, `controller-runtime` manager, RBAC markers, mutating/validating webhooks. |
| 06 | [Building Enterprise CLIs with Cobra & Viper](./06-Building-Enterprise-CLIs-with-Cobra-and-Viper.md) | Command hierarchy, POSIX flags, environment binding via Viper, table/JSON/YAML formatting. |
| 07 | [Real-World Scenarios & Outage Post-Mortems](./07-Real-World-Scenarios.md) | Production disasters: Hot loop crashing `kube-apiserver`, missing indexer memory leak, stuck finalizer. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Debugging client-side throttling (`QPS exceeded`), RBAC authorization denial, webhook failure policies. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 senior Platform Engineer / SRE interview questions on Kubernetes controllers and client-go. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Real-time Pod Informer; Lab 2: Idempotent Reconcile loop; Lab 3: Production Cobra CLI tool. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density cheat sheet for client-go types, controller-runtime markers, and Cobra boilerplate. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Testing & Quality for DevOps](../08-Testing-and-Quality-for-DevOps-Code/README.md) | [README](./README.md) | [01 - Kubernetes API Machinery](./01-Kubernetes-API-Machinery-and-client-go-Architecture.md) |
