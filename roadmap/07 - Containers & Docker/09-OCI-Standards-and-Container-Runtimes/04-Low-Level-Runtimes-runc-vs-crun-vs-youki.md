# 04 - Low-Level Runtimes: runc vs crun vs youki

## 1. High-Level vs Low-Level Runtimes

- **High-Level Runtimes (containerd, CRI-O, Docker)**: Pull images, unpack layers, manage network endpoints, supervise process lifecycles.
- **Low-Level Runtimes (runc, crun, youki)**: Receive an OCI root filesystem and `config.json` bundle, invoke Linux kernel system calls (`clone`, `unshare`, `pivot_root`, `setns`), configure cgroups, launch the process, and exit.

---

## 2. `runc` vs `crun` Performance Benchmark

- **`runc`**: The OCI reference implementation written in **Go**. Because Go contains a runtime and garbage collector, spawning a container requires ~15MB RAM and has a minor initialization delay.
- **`crun`**: High-performance implementation written in **pure C**. Spawns containers in **microseconds** with negligible memory footprint (~1MB), ideal for high-churn serverless workloads and WebAssembly (Wasm).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - CRI O The Lightweight Kubernetes Runtime](./03-CRI-O-The-Lightweight-Kubernetes-Runtime.md) | [Index](../../../README.md) | [05 - Sandboxed and MicroVM Runtimes gVisor and Kata →](./05-Sandboxed-and-MicroVM-Runtimes-gVisor-and-Kata.md) |
