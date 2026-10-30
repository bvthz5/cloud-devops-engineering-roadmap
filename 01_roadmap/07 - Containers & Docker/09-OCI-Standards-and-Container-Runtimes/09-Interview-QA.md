# 09 - Interview Questions & Architectural Scenarios

### Q1: What was Dockershim and why did Kubernetes deprecate and remove it?
**Answer**: Dockershim was a translation shim embedded in the Kubelet that translated Kubernetes CRI requests into Docker Engine REST API calls. It introduced unnecessary memory overhead, CPU latency, and maintenance burdens. Modern Kubernetes nodes communicate directly with standard CRI runtimes (`containerd` or `CRI-O`), eliminating Dockershim and the Docker daemon entirely.

### Q2: What is the architectural difference between a high-level runtime and a low-level runtime?
**Answer**: High-level runtimes (`containerd`, `CRI-O`) manage network plumbing, pull and unpack OCI images, and handle the CRI API. Low-level runtimes (`runc`, `crun`) are single-purpose execution binaries that configure kernel namespaces, cgroups, and capabilities from an OCI bundle and launch the container process.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
