# 12. Modern Web Servers: Caddy, IIS & lighttpd

## 1. Caddy Server
- **Language & Runtime:** Written in Go; single statically compiled binary.
- **Key Feature:** **Automatic HTTPS by default** using Let's Encrypt / ZeroSSL with zero manual Certbot configuration.
- **Caddyfile Example:**
  ```caddy
  example.com {
      reverse_proxy localhost:8080
  }
  ```

## 2. Microsoft IIS (Internet Information Services)
- **OS Platform:** Native Windows Server web server engine.
- **Architecture:** Worker process model (`w3wp.exe`) organized into isolated **Application Pools**.
- **Use Cases:** ASP.NET Core applications, Active Directory integrated authentication, Windows enterprise infrastructures.

## 3. lighttpd ("lighty")
- **Focus:** Ultra-lightweight footprint, single-threaded event-driven server.
- **Use Cases:** Embedded systems, IoT devices, resource-constrained Virtual Private Servers (VPS).
