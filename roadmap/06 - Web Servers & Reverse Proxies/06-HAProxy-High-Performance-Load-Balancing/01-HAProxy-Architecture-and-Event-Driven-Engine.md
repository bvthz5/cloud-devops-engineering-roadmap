# 01 - HAProxy Architecture and Event-Driven Engine

## 1. The Single-Process Event-Driven Model

HAProxy is engineered in C with extreme mechanical sympathy for modern CPU caches and Linux kernel networking. Unlike multi-process servers, HAProxy runs as a **single process** utilizing an event-driven, non-blocking I/O loop (`epoll` on Linux).

```text
[ Incoming TCP Sockets ] ──► [ epoll Event Queue ]
                                     │
                                     ▼
        ┌────────────────────────────────────────────────────────┐
        |          HAProxy Multi-Threaded Engine (nbthread)      |
        |  Thread 1 (epoll) │ Thread 2 (epoll) │ Thread N (epoll)|
        └────────────────────────────┬───────────────────────────┘
                                     │
                                     ▼
        [ Zero-Copy TCP Splicing: kernel-to-kernel socket pipe ]
                                     │
                                     ▼
                        [ Backend Upstream Fleet ]
```

### Key Architectural Advantages
1. **Zero-Copy TCP Splicing (`splice()` syscall)**: In Layer 4 TCP proxy mode, data flows directly from the client network socket to the backend network socket inside kernel space, completely avoiding user-space memory copies!
2. **Deterministic Memory Allocation**: HAProxy pre-allocates connection buffers at startup, guaranteeing it will never crash due to memory fragmentation or heap exhaustion during traffic spikes.
3. **Lockless Multi-Threading**: In HAProxy 2.x+, threads process connections concurrently using lock-free data structures and thread-local runqueues.

---

## 2. Global Tuning (`nbthread` and `maxconn`)

```haproxy
# /etc/haproxy/haproxy.cfg
global
    log /dev/log local0 info
    log /dev/log local1 notice
    chroot /var/lib/haproxy
    user haproxy
    group haproxy
    daemon

    # Automatically spawn one thread per available CPU core
    nbthread 4
    cpu-map auto:1/1-4 0-3

    # Global connection limit
    maxconn 100000

    # Runtime management socket
    stats socket /run/haproxy/admin.sock mode 660 level admin expose-fd listeners
    stats timeout 30s
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (05-Caching-and-Rate-Limiting)](../05-Caching-and-Rate-Limiting/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Configuration Structure Global Defaults Frontend Backend →](./02-Configuration-Structure-Global-Defaults-Frontend-Backend.md) |
