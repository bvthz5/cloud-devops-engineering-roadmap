# 06 - Observability: OpenTelemetry and Envoy Access Logs

## 1. The Envoy Stats Engine

Envoy features one of the most comprehensive telemetry engines in existence, emitting thousands of real-time metrics across three categories:
- **Downstream**: Metrics on incoming client sockets and TLS handshakes.
- **Server**: Global memory, thread execution, and uptime statistics.
- **Upstream**: Detailed connection pool, retry, circuit breaking, and latency histograms per cluster.

```bash
# Query Prometheus-formatted metrics via Envoy admin port
curl -s http://127.0.0.1:15000/stats/prometheus | grep "cluster.service_backend"
```

---

## 2. Distributed Tracing Propagation

Envoy automatically handles W3C `traceparent` and B3 headers, generating spans for every forwarded request and exporting them to OpenTelemetry collectors:

```yaml
tracing:
  provider:
    name: envoy.tracers.opentelemetry
    typed_config:
      "@type": type.googleapis.com/envoy.config.trace.v3.OpenTelemetryConfig
      grpc_service:
        envoy_grpc:
          cluster_name: otel_collector
      service_name: ingress_gateway
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - HTTP/2 & gRPC Bridging](./05-HTTP2-and-gRPC-Bridging-and-Transcoding.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
