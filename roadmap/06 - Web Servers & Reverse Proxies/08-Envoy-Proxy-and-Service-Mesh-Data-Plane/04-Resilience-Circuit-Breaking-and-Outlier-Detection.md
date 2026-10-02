# 04 - Resilience: Circuit Breaking and Outlier Detection

## 1. Outlier Detection (Passive Health Checking)

Outlier detection continuously tracks backend responses. If an upstream instance returns consecutive 5xx server errors, Envoy temporarily **ejects** the faulty instance from the load balancing pool without waiting for active health check probes:

```yaml
clusters:
  - name: resilient_backend
    connect_timeout: 0.5s
    type: STRICT_DNS
    lb_policy: ROUND_ROBIN
    outlier_detection:
      consecutive_5xx: 3               # Eject host after 3 consecutive 5xx errors
      interval: 10s                    # Evaluation window
      base_ejection_time: 30s          # Keep ejected for 30 seconds
      max_ejection_percent: 50         # Never eject more than 50% of the pool!
```

---

## 2. Circuit Breaking

Circuit breaking protects downstream clients and upstream backends from cascading collapse during overload:

```yaml
    circuit_breakers:
      thresholds:
        - priority: DEFAULT
          max_connections: 1024        # Max concurrent TCP connections to upstream
          max_pending_requests: 100    # Max requests waiting in queue for connection
          max_requests: 2048           # Max active in-flight requests
          max_retries: 3               # Max concurrent retries
```

When `max_connections` or `max_pending_requests` is exceeded, Envoy immediately returns **HTTP 503 (`overflow`)** to the client rather than letting requests pile up and crash the upstream server!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Filter Chains and Network HTTP Filters](./03-Filter-Chains-and-Network-HTTP-Filters.md) | [Index](../../../README.md) | [05 - HTTP2 and gRPC Bridging and Transcoding →](./05-HTTP2-and-gRPC-Bridging-and-Transcoding.md) |
