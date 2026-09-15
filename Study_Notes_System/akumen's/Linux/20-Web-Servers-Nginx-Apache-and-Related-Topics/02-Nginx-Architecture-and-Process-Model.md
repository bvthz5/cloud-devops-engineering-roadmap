# 2. Nginx Architecture & Process Model

## Master-Worker Architecture

Nginx uses a non-threaded, event-driven master-worker process architecture designed for ultra-high concurrency and low memory overhead:

```text
               [ Nginx Master Process ] (runs as root)
               │ (Reads config, binds ports, manages workers)
               ├───────────────────────┬───────────────────────┐
               ▼                       ▼                       ▼
    [ Worker Process 1 ]    [ Worker Process 2 ]    [ Worker Process N ]
     (Non-blocking Event)   (Non-blocking Event)   (Non-blocking Event)
               │                       │                       │
               └───────────────────────┼───────────────────────┘
                                       ▼
                       [ Epoll / Kqueue Event Loop ]
                                       │
                      ┌────────────────┴────────────────┐
                      ▼                                 ▼
             [ Client Request A ]              [ Client Request B ]
```

## Core Components

1. **Master Process:**
   - Runs with `root` privileges to bind privileged ports (80, 443).
   - Reads and validates `/etc/nginx/nginx.conf`.
   - Spawns worker processes and handles signals (`nginx -s reload`, `nginx -s stop`).
   - Performs zero-downtime configuration reloads without dropping active client connections.

2. **Worker Processes:**
   - Run as non-privileged user (e.g., `www-data` or `nginx`).
   - Usually configured equal to the number of CPU cores (`worker_processes auto;`).
   - Process thousands of concurrent connections using an asynchronous event loop (`epoll` on Linux, `kqueue` on BSD/macOS).

3. **Cache Loader & Cache Manager:**
   - Auxiliary processes spawned to maintain disk-backed proxy caches (`proxy_cache`).

## Asynchronous Non-Blocking Event Loop vs Process-Per-Request

- **Process/Thread-per-Request (Legacy):** Blocks an entire OS thread waiting for disk or network I/O. Memory scales linearly with concurrent connections (1,000 requests = 1,000 threads).
- **Asynchronous Event Loop (Nginx):** A single worker process handles thousands of connections in a single thread. When I/O blocks, the event loop immediately moves to process ready events from other sockets. Memory footprint remains minimal and flat.
