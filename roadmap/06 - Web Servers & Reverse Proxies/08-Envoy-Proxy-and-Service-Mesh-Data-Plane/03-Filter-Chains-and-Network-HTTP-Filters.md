# 03 - Filter Chains and Network/HTTP Filters

## 1. Filter Architecture

Envoy's processing pipeline is assembled from modular **Filter Chains**:

```text
[ Downstream Client ]
         │
         ▼
[ Listener ] ──► [ Listener Filters ] (e.g., TLS Inspector, Original Destination)
                       │
                       ▼
                 [ Network Filters ]  (e.g., TCP Proxy, HTTP Connection Manager)
                       │
                       ▼ (If HTTP Connection Manager selected)
                 [ HTTP Filters ]
                   1. CORS Filter
                   2. Rate Limit Filter
                   3. JWT Auth Filter
                   4. Router Filter (Terminal filter - dispatches to Upstream)
                       │
                       ▼
                 [ Upstream Cluster ]
```

---

## 2. Minimal Static Envoy Configuration (`envoy.yaml`)

```yaml
static_resources:
  listeners:
    - name: listener_0
      address:
        socket_address:
          address: 0.0.0.0
          port_value: 10000
      filter_chains:
        - filters:
            - name: envoy.filters.network.http_connection_manager
              typed_config:
                "@type": type.googleapis.com/envoy.extensions.filters.network.http_connection_manager.v3.HttpConnectionManager
                stat_prefix: ingress_http
                route_config:
                  name: local_route
                  virtual_hosts:
                    - name: local_service
                      domains: ["*"]
                      routes:
                        - match:
                            prefix: "/"
                          route:
                            cluster: service_backend
                http_filters:
                  - name: envoy.filters.http.router
                    typed_config:
                      "@type": type.googleapis.com/envoy.extensions.filters.http.router.v3.Router

  clusters:
    - name: service_backend
      connect_timeout: 0.25s
      type: STRICT_DNS
      lb_policy: ROUND_ROBIN
      load_assignment:
        cluster_name: service_backend
        endpoints:
          - lb_endpoints:
              - endpoint:
                  address:
                    socket_address:
                      address: 127.0.0.1
                      port_value: 8080
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - xDS Dynamic Configuration APIs](./02-xDS-Dynamic-Configuration-APIs.md) | [README](./README.md) | [04 - Resilience & Circuit Breaking](./04-Resilience-Circuit-Breaking-and-Outlier-Detection.md) |
