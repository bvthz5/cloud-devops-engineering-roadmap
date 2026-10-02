# 08 - Envoy Proxy and Service Mesh Data Plane

Envoy is an open-source, high-performance C++ edge and service proxy designed for cloud-native architectures. Serving as the universal data plane for modern service meshes (Istio, Consul Connect, Linkerd) and Kubernetes Gateway API implementations (Envoy Gateway), Envoy provides dynamic configuration via gRPC streaming APIs (xDS), advanced traffic steering, out-of-band resilience (circuit breaking, outlier detection), and rich distributed tracing.

---

## Module Index

| # | Topic | Description |
|---|---|---|
| 01 | [Envoy Architecture, Threading & Event Model](./01-Envoy-Architecture-Threading-and-Event-Model.md) | C++ architecture, main thread vs worker threads, non-blocking libevent, Envoy vs Nginx. |
| 02 | [xDS Dynamic Configuration APIs](./02-xDS-Dynamic-Configuration-APIs.md) | Universal data plane APIs: LDS (Listeners), RDS (Routes), CDS (Clusters), EDS (Endpoints). |
| 03 | [Filter Chains & Network/HTTP Filters](./03-Filter-Chains-and-Network-HTTP-Filters.md) | Listener filters, Network filters, HTTP Connection Manager (HCM), Router filter, Wasm filters. |
| 04 | [Resilience: Circuit Breaking & Outlier Detection](./04-Resilience-Circuit-Breaking-and-Outlier-Detection.md) | Connection pool limits, pending requests, consecutive 5xx outlier ejection, active health checking. |
| 05 | [HTTP/2 & gRPC Bridging and Transcoding](./05-HTTP2-and-gRPC-Bridging-and-Transcoding.md) | gRPC-Web, JSON-to-gRPC transcoding with protobuf descriptors, HTTP/2 multiplexing. |
| 06 | [Observability: OpenTelemetry & Envoy Access Logs](./06-Observability-OpenTelemetry-and-Envoy-Access-Logs.md) | Stats engine (statsd/Prometheus), W3C trace propagation, custom access log formatting. |
| 07 | [Real-World Scenarios](./07-Real-World-Scenarios.md) | Enterprise post-mortems: Control plane disconnect CDS drop, memory leak in Wasm filter. |
| 08 | [Troubleshooting & Diagnostic Runbooks](./08-Troubleshooting.md) | Envoy admin port `:15000` (`/config_dump`, `/clusters`, `/stats`), changing log level dynamically. |
| 09 | [Interview Questions & Architectural Scenarios](./09-Interview-QA.md) | 10 Senior Platform/SRE questions on Envoy xDS, Service Mesh data plane, and circuit breaking. |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Lab 1: Static Envoy proxy with Docker; Lab 2: Circuit breaking lab; Lab 3: Admin diagnostics. |
| 11 | [Multiple-Choice Assessment (MCQ)](./11-MCQ.md) | 10 technical scenario MCQs with deep architectural rationales. |
| 12 | [Quick-Revision & Enterprise Cheat Sheet](./12-Quick-Revision.md) | High-density reference for envoy.yaml structure, admin endpoints, and xDS protocols. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Traefik Cloud-Native Proxy](../07-Traefik-Cloud-Native-Reverse-Proxy/README.md) | [README](./README.md) | [01 - Envoy Architecture](./01-Envoy-Architecture-Threading-and-Event-Model.md) |
