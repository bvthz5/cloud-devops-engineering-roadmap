# 09 - Interview Questions & Scenarios

### Q1: Explain Terraform's architecture — how does the core binary interact with providers?
**Answer:**
Terraform Core is a single Go binary that parses HCL, builds a DAG of resources, and manages state. Providers are separate plugin binaries that communicate with Core via gRPC (plugin protocol v5/v6). During `terraform init`, Core downloads the required provider binaries from the registry and stores them in `.terraform/providers/`. When Core needs to create, read, update, or delete a resource, it sends a gRPC call to the appropriate provider plugin, which translates it into cloud API calls.

---

### Q2: What is the dependency lock file and why should it be committed to Git?
**Answer:**
`.terraform.lock.hcl` records the exact provider versions and their cryptographic hashes. Committing it ensures all team members and CI pipelines use identical provider binaries, preventing "works on my machine" issues caused by provider version drift.

---

### Q3: How does Terraform's DAG determine execution order and parallelism?
**Answer:**
Terraform builds a Directed Acyclic Graph where nodes are resources and edges are dependencies. Resources with no interdependencies are created in parallel (default: 10 concurrent operations). The DAG is rebuilt on every `plan` and `apply` to account for configuration changes.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
