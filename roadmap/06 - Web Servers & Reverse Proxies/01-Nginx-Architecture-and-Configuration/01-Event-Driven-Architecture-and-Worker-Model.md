# 01 - Event-Driven Architecture and Worker Model

## 1. Thread-Per-Connection vs Event-Driven Architecture

Traditional web servers (like Apache with `mpm_prefork` or naive socket servers) allocate a dedicated operating system process or thread to every incoming HTTP client connection. 

```text
Traditional Thread-Per-Connection Model:
Client 1 ──► [ OS Thread 1 (Blocks on Disk/Network I/O) ] ──► Consumes 2-8MB RAM
Client 2 ──► [ OS Thread 2 (Blocks on Disk/Network I/O) ] ──► High Context Switching
Client N ──► [ OS Thread N ... Exhausts Kernel Resources ] ──► System Collapse (C10K Fail)

Nginx Asynchronous Event-Driven Model:
Client 1 ──┐
Client 2 ──┼──► [ epoll / kqueue Event Queue ] ──► [ Single Worker Process Event Loop ]
Client N ──┘                                        (Non-blocking I/O: 1 thread serves
                                                     thousands of concurrent requests)
```

### The C10K Problem Solved
Under 10,000 concurrent idle or slow connections (e.g., keepalive, slow mobile networks):
- **Threaded model**: 10,000 threads × 4MB stack = 40GB RAM just for thread stacks, accompanied by devastating kernel CPU context-switching overhead.
- **Nginx event-driven model**: Single worker process consumes ~10-20MB RAM and processes ready events via Linux **`epoll`** (or BSD **`kqueue`**), achieving $O(1)$ event dispatch complexity regardless of total idle connections.

---

## 2. The Master-Worker Process Hierarchy

Nginx operates using a privileged **Master Process** and one or more unprivileged **Worker Processes**.

```text
               +------------------------------------------------+
               |             Nginx Master Process               |
               |        (Runs as root; PID 1 in container)      |
               +-----------------------+------------------------+
                                       |
                   ┌───────────────────┼───────────────────┐
                   | (Forks & Monitors)| (Sends Signals)   |
                   ▼                   ▼                   ▼
           [ Worker 1 (epoll) ]  [ Worker 2 (epoll) ]  [ Cache Loader / Manager ]
           (Runs as 'nginx')     (Runs as 'nginx')     (Processes on-disk cache)
```

### Roles and Responsibilities
1. **Master Process**:
   - Reads and validates configuration files (`nginx -t`).
   - Binds to privileged network sockets (ports 80 and 443).
   - Forks, monitors, and restarts worker processes if they crash.
   - Manages seamless, zero-downtime configuration reloads (`HUP` signal) and binary upgrades (`USR2` signal).
2. **Worker Processes**:
   - Run as an unprivileged user (e.g., `nginx` or `www-data`) for security isolation.
   - Accept incoming client connections on listening sockets.
   - Execute HTTP parsing, TLS encryption/decryption, URL routing, and upstream proxying.
   - Run an infinite event loop using OS multiplexing primitives (`epoll_wait`).

---

## 3. Worker Sizing and CPU Pinning

In high-throughput environments, worker processes should match the number of available physical/logical CPU cores to eliminate CPU context switching:

```nginx
# /etc/nginx/nginx.conf
user nginx;
worker_processes auto; # Automatically detects available CPU cores

# Explicitly pin worker processes to specific CPU cores (affinity)
worker_cpu_affinity auto;

# Maximum number of open file descriptors per worker process
worker_rlimit_nofile 65535;

events {
    # Maximum concurrent connections per worker
    worker_connections 16384;
    
    # Use efficient Linux epoll event notification mechanism
    use epoll;
    
    # Accept as many connections as possible upon receiving a notification
    multi_accept on;
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Section (05 - Programming & Scripting)](../../05%20-%20Programming%20%26%20Scripting/09-Kubernetes-Client-Go-and-Custom-Controllers/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Configuration File Hierarchy and Contexts →](./02-Configuration-File-Hierarchy-and-Contexts.md) |
