# 10 - Hands-On Practice Labs

## Lab 1: Deploying Envoy with Outlier Detection in Docker

### Objective
Deploy an Envoy proxy in Docker that routes traffic to two backend containers with automated outlier ejection when a backend fails.

### Implementation (`envoy.yaml`)
```yaml
admin:
  address:
    socket_address: { address: 0.0.0.0, port_value: 15000 }

static_resources:
  listeners:
    - name: ingress
      address:
        socket_address: { address: 0.0.0.0, port_value: 80 }
      filter_chains:
        - filters:
            - name: envoy.filters.network.http_connection_manager
              typed_config:
                "@type": type.googleapis.com/envoy.extensions.filters.network.http_connection_manager.v3.HttpConnectionManager
                stat_prefix: ingress_http
                route_config:
                  virtual_hosts:
                    - name: backend
                      domains: ["*"]
                      routes:
                        - match: { prefix: "/" }
                          route: { cluster: app_cluster }
                http_filters:
                  - name: envoy.filters.http.router
                    typed_config:
                      "@type": type.googleapis.com/envoy.extensions.filters.http.router.v3.Router

  clusters:
    - name: app_cluster
      connect_timeout: 1s
      type: STRICT_DNS
      lb_policy: ROUND_ROBIN
      load_assignment:
        cluster_name: app_cluster
        endpoints:
          - lb_endpoints:
              - endpoint: { address: { socket_address: { address: app1, port_value: 80 } } }
              - endpoint: { address: { socket_address: { address: app2, port_value: 80 } } }
      outlier_detection:
        consecutive_5xx: 2
        interval: 5s
        base_ejection_time: 15s
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
