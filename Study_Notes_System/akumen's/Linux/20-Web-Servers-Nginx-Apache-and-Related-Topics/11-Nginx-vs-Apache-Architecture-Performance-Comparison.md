# 11. Nginx vs Apache: Architecture & Performance Comparison

| Feature / Criteria | Nginx | Apache HTTP Server |
| --- | --- | --- |
| **Core Architecture** | Asynchronous Event-Driven Reactor | Process/Thread-based MPMs (Prefork, Worker, Event) |
| **Static File Serving** | Blazing fast, minimal CPU & memory overhead | Fast, but higher memory footprint per connection |
| **Dynamic Content Handling** | Indirect (Passes via FastCGI/uWSGI/Proxy) | Direct (Embedded modules like `mod_php`) or Proxy |
| **Directory Config Overrides** | No `.htaccess` support (Centralized config only) | Supports `.htaccess` (Per-directory, with I/O overhead) |
| **Memory Footprint** | Extremely low and flat under high concurrency | Higher memory consumption under heavy load |
| **Module Ecosystem** | Compiled at build time (or dynamic modules) | Highly flexible dynamic DSO (`mod_*`) runtime loading |
| **Primary Industry Role** | Reverse Proxy, API Gateway, Load Balancer | Legacy web hosting, complex directory overrides |

## Summary Architecture Decision Guide
- Choose **Nginx** for reverse proxying, high-concurrency static/API serving, TLS termination, and Kubernetes ingress controllers.
- Choose **Apache** when requiring `.htaccess` directory-level user control (e.g., shared hosting), embedded legacy language modules, or specific Apache modules (`mod_security2`).
