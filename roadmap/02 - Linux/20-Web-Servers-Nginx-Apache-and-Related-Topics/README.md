# Topic 21 — Web Servers: Nginx, Apache & Related Topics

## Objective
Master the web server ecosystem, HTTP/HTTPS protocols, reverse proxying, load balancing, process architectures (Nginx event loop vs Apache MPMs), TLS/SSL termination, caching, performance optimization, security hardening, app runtime integration (PHP-FPM, Node.js, Gunicorn/Uvicorn, Tomcat), HAProxy, Caddy, IIS, production troubleshooting, and DevOps/Kubernetes ingress connections.

## Module Structure

1. **`01-Web-Server-Fundamentals-HTTP-HTTPS-DNS-Ports.md`**: Client-server architecture, HTTP/1.1 vs HTTP/2 vs HTTP/3, request/response lifecycle, DNS resolution, standard ports (80, 443, 8080).
2. **`02-Nginx-Architecture-and-Process-Model.md`**: Master-worker architecture, asynchronous event-driven reactor model, non-blocking I/O.
3. **`03-Nginx-Configuration-Structure-and-Server-Blocks.md`**: `nginx.conf` layout, contexts (`main`, `events`, `http`, `server`, `location`), directives, location matching rules (`=`, `^~`, `~`, `~*`).
4. **`04-Nginx-Reverse-Proxy-Configuration.md`**: `proxy_pass`, header forwarding (`X-Forwarded-For`, `X-Real-IP`, `Host`), path rewriting, buffer settings.
5. **`05-Nginx-Load-Balancing-Algorithms-and-Health-Checks.md`**: Round-Robin, Least Connections, IP Hash, Weighted distribution, active vs passive health checks.
6. **`06-TLS-HTTPS-SSL-Certificates-and-Certbot.md`**: PKI, SSL/TLS handshake, Let's Encrypt, Certbot auto-renewal, OCSP stapling, cipher suites, HSTS.
7. **`07-Caching-Compression-Gzip-Brotli-and-HTTP-Headers.md`**: `proxy_cache`, `gzip`/`brotli` compression, cache-control headers, security headers (CSP, X-Frame-Options, HSTS).
8. **`08-WebSockets-HTTP2-HTTP3-and-gRPC-Proxying.md`**: Upgrade headers for WebSockets, HTTP/2 multiplexing, HTTP/3 (QUIC/UDP), gRPC `grpc_pass`.
9. **`09-Apache-HTTP-Server-Architecture-and-MPMs.md`**: Apache process models: Prefork, Worker, Event MPMs; `.htaccess` performance implications.
10. **`10-Apache-Virtual-Hosts-and-Reverse-Proxy-Mod_Proxy.md`**: Name-based & IP-based virtual hosts (`<VirtualHost>`), `mod_proxy`, `mod_proxy_http`, `mod_rewrite`.
11. **`11-Nginx-vs-Apache-Architecture-Performance-Comparison.md`**: Event-driven vs thread/process-per-request, memory footprint, static vs dynamic content handling, `.htaccess` vs centralized config.
12. **`12-Modern-Web-Servers-Caddy-IIS-lighttpd.md`**: Caddy (Automatic HTTPS, Caddyfile, Go runtime), Microsoft IIS (Windows, App Pools), lighttpd (lightweight footprint).
13. **`13-Application-Servers-and-Containers-Tomcat-HAProxy.md`**: Apache Tomcat (Jakarta Servlet/JSP container), HAProxy (high-performance L4/L7 load balancer).
14. **`14-Static-vs-Dynamic-App-Execution-PHP-FPM-Node-Python.md`**: Static file serving vs Dynamic app gateways: FastCGI (`PHP-FPM`), reverse proxying Node.js, Python WSGI/ASGI (`Gunicorn`/`Uvicorn`), Java (`Tomcat`).
15. **`15-Logging-Formats-Log-Analysis-Access-and-Error-Logs.md`**: Combined log format, custom JSON logging, logrotate, parsing access/error logs (`grep`, `awk`, `goaccess`).
16. **`16-HTTP-Status-Codes-and-Troubleshooting-502-504-403.md`**: 2xx, 3xx, 4xx (403, 404), 5xx (500, 502 Bad Gateway, 504 Gateway Timeout); root cause diagnosis.
17. **`17-Web-Server-Security-Hardening-Best-Practices.md`**: Disabling server tokens/version headers, rate limiting (`limit_req`), request size caps (`client_max_body_size`), DDoS protection, ModSecurity WAF.
18. **`18-Hands-On-Lab-Ubuntu-Nginx-Reverse-Proxy-Setup.md`**: Step-by-step setup of Nginx as a TLS-terminated reverse proxy to a Node.js/Python backend.
19. **`19-Hands-On-Lab-Ubuntu-Apache-Virtual-Hosts-Setup.md`**: Step-by-step setup of Apache with Event MPM, multiple Virtual Hosts, and `mod_rewrite`.
20. **`20-Production-Web-Server-Deployment-Checklist.md`**: Pre-flight production readiness verification across security, performance, TLS, logging, and monitoring.
21. **`21-Web-Server-Interview-Questions-and-Answers.md`**: Scenario-based & technical interview Q&A for DevOps/SRE roles.
22. **`22-MCQs-and-Quick-Revision-Quiz.md`**: 10 multiple-choice questions with answer keys and explanations.
23. **`23-Scenario-Based-Troubleshooting-Challenges.md`**: Real-world incident challenges (502 Gateway Timeout, SSL handshake failures, high CPU spikes).
24. **`24-DevOps-Cloud-and-Kubernetes-Web-Server-Connections.md`**: Cloud Load Balancers (AWS ALB, GCP HTTP(S) LB), Nginx Ingress Controller in Kubernetes, Envoy, Service Meshes.
25. **`25-Web-Server-Command-and-Config-Cheat-Sheet.md`**: Concise syntax and command reference for Nginx, Apache, Certbot, and troubleshooting.
26. **`SOURCE.md`**: Official documentation links (Nginx, Apache, Caddy, Tomcat, HAProxy).
