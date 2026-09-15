# 13. Application Servers & Containers: Tomcat & HAProxy

## 1. Web Server vs Application Server / Container

- **Web Server (Nginx, Apache):** Handles HTTP transport, static assets, TLS termination, reverse proxying.
- **Application Server / Servlet Container (Tomcat):** Executes server-side business logic code (Jakarta Servlets, JSPs, enterprise Java code).

```text
[ Client ] ──► [ Nginx (Reverse Proxy) ] ── (AJP / HTTP) ──► [ Apache Tomcat ] ──► [ Java Code / DB ]
```

## 2. Apache Tomcat Overview
- **Role:** Open-source Jakarta Servlet, Jakarta Expression Language, and WebSocket implementation.
- **Ports:** 8080 (HTTP Connector), 8009 (AJP Connector), 8005 (Shutdown).
- **Deployment Artifacts:** `.war` (Web Application Archive) files deployed to `webapps/` directory.

## 3. HAProxy (High Availability Proxy)
- **Role:** Specialized, event-driven Layer 4 (TCP) and Layer 7 (HTTP) load balancer and proxy.
- **Key Capabilities:** High-performance session stickiness, advanced health checks, statistics dashboard, SSL termination.
- **HAProxy Config Example (`haproxy.cfg`):**
  ```haproxy
  frontend http_in
      bind *:80
      default_backend web_servers

  backend web_servers
      balance roundrobin
      server web1 192.168.1.10:80 check
      server web2 192.168.1.11:80 check
  ```
