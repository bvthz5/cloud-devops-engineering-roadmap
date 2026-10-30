# MCQ - GitOps Principles & Workflow

> **Module**: GitOps Principles & Workflow

---

### Question 1
Which OpenGitOps principle states that software agents must continuously observe live cluster state and automatically correct discrepancies?
- [ ] A) Push-Based Execution
- [x] B) Continuous Reconciliation
- [ ] C) Manual Intervention
- [ ] D) Imperative Scripting

*Explanation: Continuous Reconciliation ensures agents audit and self-heal live cluster drift back to Git state.*

---

### Question 2
What ArgoCD feature allows developers to specify the exact sequence of resource creation (e.g., creating Namespaces before Deployments)?
- [ ] A) Matrix Generators
- [x] B) Sync Waves
- [ ] C) Health Probes
- [ ] D) Ingress Overlays

*Explanation: Sync Waves order resource deployment by assigning numeric wave annotations.*

---

### Question 3
How does Kustomize apply environment-specific configuration changes without using template parameters?
- [ ] A) By compiling Groovy scripts
- [x] B) By applying declarative overlay patches onto base manifest files
- [ ] C) By executing shell scripts inside pods
- [ ] D) By importing Helm values files

*Explanation: Kustomize uses template-free overlay patches to modify base manifests per environment.*

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
