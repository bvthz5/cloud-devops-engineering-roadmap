# 01 - Open Container Initiative (OCI) Specifications

## 1. What is the Open Container Initiative?

Established in 2015 by Docker and industry leaders under the Linux Foundation, the OCI establishes vendor-neutral open specifications for container formats and runtimes:

```text
+-----------------------------------------------------------------------------------+
|                           The 3 OCI Specifications                                |
+-----------------------+-----------------------------------------------------------+
| Specification         | Core Responsibility                                       |
+-----------------------+-----------------------------------------------------------+
| **OCI Image Spec**    | Defines image layout, manifests, configuration JSON, and  |
|                       | layer tarballs on disk.                                   |
+-----------------------+-----------------------------------------------------------+
| **OCI Runtime Spec**  | Defines the lifecycle and bundle format (`config.json` +  |
|                       | rootfs) to unpack and execute an isolated container.     |
+-----------------------+-----------------------------------------------------------+
| **OCI Distribution**  | Defines the standardized HTTP API for pushing, pulling,   |
|                       | and querying container images across remote registries.   |
+-----------------------+-----------------------------------------------------------+
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - containerd Architecture & CRI](./02-Containerd-Architecture-and-CRI-Plugin.md) |
