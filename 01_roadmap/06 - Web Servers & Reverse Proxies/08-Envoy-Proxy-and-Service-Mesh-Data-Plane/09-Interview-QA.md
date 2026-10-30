# 09 - Interview Questions & Architectural Scenarios

### Q1: What makes Envoy uniquely suited for cloud-native microservices compared to traditional Nginx?
**Answer**: Envoy was designed from inception for dynamic, ephemeral microservices. It features dynamic gRPC-based xDS APIs for live configuration streaming, native HTTP/2 and gRPC support, built-in distributed tracing (OpenTelemetry/W3C), and out-of-band resilience features like circuit breaking and outlier ejection built directly into its core C++ threading model.

### Q2: Explain the four primary xDS APIs and their roles.
**Answer**: LDS (Listener Discovery Service) configures network ports and filter chains; RDS (Route Discovery Service) configures HTTP routes and virtual hosts; CDS (Cluster Discovery Service) configures upstream service pools and load balancing algorithms; EDS (Endpoint Discovery Service) streams live IP addresses and ports of backend containers/pods.

### Q3: What is the difference between Circuit Breaking and Outlier Detection in Envoy?
**Answer**: Circuit Breaking is **proactive**: it sets strict limits on maximum concurrent connections, requests, or retries, immediately rejecting traffic (HTTP 503) when limits are breached to prevent backend saturation. Outlier Detection is **reactive/passive**: it monitors actual response codes (consecutive 5xx errors) and temporarily ejects failing hosts from the load balancing pool.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
