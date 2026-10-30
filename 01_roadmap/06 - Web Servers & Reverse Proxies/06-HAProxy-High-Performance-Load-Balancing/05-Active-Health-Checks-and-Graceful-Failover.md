# 05 - Active Health Checks and Graceful Failover

## 1. Active Health Checking Mechanics

Unlike open-source Nginx which relies on passive in-band errors, HAProxy includes enterprise-grade **active out-of-band health checking**:

```haproxy
backend app_servers
    balance roundrobin
    mode http

    # Active HTTP probe:
    # Sends: GET /healthz HTTP/1.1
    # Host: healthcheck.internal
    option httpchk GET /healthz HTTP/1.1\r\nHost:\ healthcheck.internal
    
    # Expect HTTP 200 or 204
    http-check expect status 200,204

    # inter 2s: Check every 2 seconds
    # fall 3: Mark DOWN after 3 consecutive failures (6 seconds total)
    # rise 2: Mark UP after 2 consecutive successes
    default-server inter 2s fall 3 rise 2

    server app1 10.0.1.10:8080 check
    server app2 10.0.1.11:8080 check
    server app3 10.0.1.12:8080 check
    
    # Cold Standby Disaster Recovery Server
    server dr_app 10.0.99.10:8080 check backup
```

---

## 2. Graceful Drain for Zero-Downtime Deployments

Before taking a backend server down for software upgrades, it should be set to `DRAIN` state:
- New connections are stopped.
- Existing active sessions and users with valid session cookies continue to be served until their sessions expire.

```bash
# Send DRAIN command via runtime socket
echo "set server app_servers/app1 state drain" | socat stdio /run/haproxy/admin.sock

# Once traffic drops to 0, mark fully MAINT:
echo "set server app_servers/app1 state maint" | socat stdio /run/haproxy/admin.sock
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Stick Tables and Advanced Rate Limiting](./04-Stick-Tables-and-Advanced-Rate-Limiting.md) | [Index](../../../README.md) | [06 - Dynamic Reconfiguration Runtime API and Data Plane API →](./06-Dynamic-Reconfiguration-Runtime-API-and-Data-Plane-API.md) |
