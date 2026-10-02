# 06 - Observability: Tracing, Metrics, and Dashboard

## 1. Prometheus Metrics Export

Traefik provides native Prometheus metrics integration out of the box:

```yaml
# traefik.yml
metrics:
  prometheus:
    addEntryPointsLabels: true
    addRoutersLabels: true
    addServicesLabels: true
    buckets:
      - 0.1
      - 0.3
      - 1.2
      - 5.0
```

Metrics are exposed on `:8080/metrics` or a dedicated internal entrypoint.

---

## 2. OpenTelemetry & Jaeger Distributed Tracing

```yaml
tracing:
  otlp:
    grpc:
      endpoint: "otel-collector:4317"
      insecure: true
```

Traefik automatically propagates W3C `traceparent` headers to downstream microservices, allowing SREs to trace an incoming HTTP request through Traefik, application microservices, and databases in Jaeger or Datadog.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Automated Lets Encrypt TLS Management](./05-Automated-Lets-Encrypt-TLS-Management.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
