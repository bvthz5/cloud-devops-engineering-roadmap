# 02 - Configuration Structure: Global, Defaults, Frontend & Backend

## 1. The 4 Fundamental Sections

An HAProxy configuration file (`haproxy.cfg`) is partitioned into four clear, logical sections:

```text
1. global:
   Process-wide, OS-level settings (user, group, logging, SSL ciphers, runtime socket).

2. defaults:
   Baseline parameters inherited by all subsequent frontends and backends (mode, timeouts).

3. frontend:
   Defines how HAProxy accepts incoming connections (bind IP/port, SSL termination, ACL rules).

4. backend:
   Defines the upstream server pool (servers, load balancing algorithm, health checks).

* listen: (Shorthand combining a frontend and backend into a single block).
```

---

## 2. Complete Production Configuration

```haproxy
global
    log /dev/log local0
    user haproxy
    group haproxy
    daemon
    maxconn 50000

defaults
    log     global
    mode    http
    option  httplog
    option  dontlognull
    timeout connect 5000ms
    timeout client  50000ms
    timeout server  50000ms

# Frontend: Ingress on port 80 and 443
frontend http_in
    bind *:80
    bind *:443 ssl crt /etc/haproxy/certs/site.pem alpn h2,http/1.1
    
    # Redirect HTTP to HTTPS
    redirect scheme https code 301 if !{ ssl_fc }

    # Route traffic based on Host header
    acl is_api  hdr_beg(host) -i api.
    acl is_auth path_beg /auth/

    use_backend api_servers  if is_api
    use_backend auth_servers if is_auth
    default_backend web_servers

# Backend Pools
backend api_servers
    balance roundrobin
    option httpchk GET /healthz
    http-check expect status 200
    default-server inter 3s fall 2 rise 2
    server api01 10.0.1.10:8080 check maxconn 500
    server api02 10.0.1.11:8080 check maxconn 500
    server api_backup 10.0.1.99:8080 check backup

backend web_servers
    balance leastconn
    cookie SRVNAME insert indirect nocache
    server web01 10.0.1.20:80 cookie s1 check
    server web02 10.0.1.21:80 cookie s2 check
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - HAProxy Architecture and Event Driven Engine](./01-HAProxy-Architecture-and-Event-Driven-Engine.md) | [Index](../../../README.md) | [03 - Layer 4 vs Layer 7 Proxying and ACLs →](./03-Layer-4-vs-Layer-7-Proxying-and-ACLs.md) |
