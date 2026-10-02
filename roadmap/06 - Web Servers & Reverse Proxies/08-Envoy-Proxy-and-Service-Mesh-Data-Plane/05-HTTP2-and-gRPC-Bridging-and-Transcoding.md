# 05 - HTTP/2 and gRPC Bridging and Transcoding

## 1. gRPC-Web and Protocol Bridging

Modern microservices frequently communicate internally over high-speed **gRPC (HTTP/2 + Protocol Buffers)**. However, standard web browser JavaScript APIs cannot directly make native gRPC calls due to lack of low-level HTTP/2 framing control.

Envoy acts as a universal bridge via the `envoy.filters.http.grpc_web` filter:
- Accepts standard **gRPC-Web** HTTP/1.1 or HTTP/2 calls from web browsers.
- Translates them into native binary **gRPC HTTP/2** upstream calls.
- Encodes gRPC status trailers back into headers that browsers can read.

```yaml
http_filters:
  - name: envoy.filters.http.grpc_web
    typed_config:
      "@type": type.googleapis.com/envoy.extensions.filters.http.grpc_web.v3.GrpcWeb
  - name: envoy.filters.http.cors
    typed_config:
      "@type": type.googleapis.com/envoy.extensions.filters.http.cors.v3.Cors
  - name: envoy.filters.http.router
    typed_config:
      "@type": type.googleapis.com/envoy.extensions.filters.http.router.v3.Router
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Resilience & Circuit Breaking](./04-Resilience-Circuit-Breaking-and-Outlier-Detection.md) | [README](./README.md) | [06 - Observability & Access Logs](./06-Observability-OpenTelemetry-and-Envoy-Access-Logs.md) |
