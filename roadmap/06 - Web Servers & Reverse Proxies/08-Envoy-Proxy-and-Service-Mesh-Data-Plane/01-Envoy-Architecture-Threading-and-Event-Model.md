# 01 - Envoy Architecture, Threading, and Event Model

## 1. Threading Architecture: Main vs Worker Threads

Envoy uses a single-process, multi-threaded architectural model written in modern C++14/17:
- **Main Thread**: Manages the server lifecycle, accepts configuration updates via dynamic xDS management servers, initializes listeners, and manages stats and admin APIs.
- **Worker Threads**: Each worker thread binds to an event loop (`libevent`) and handles incoming client connections assigned via kernel `SO_REUSEPORT`. Once a connection is assigned to a worker, that worker handles all filtering, routing, and upstream proxying without cross-thread locking (**Thread Local Storage / TLS**).

```text
[ Incoming Connections ] ──► (SO_REUSEPORT)
                                   │
       ┌───────────────────────────┼───────────────────────────┐
       ▼                           ▼                           ▼
[ Worker Thread 1 ]         [ Worker Thread 2 ]         [ Worker Thread N ]
  - libevent loop             - libevent loop             - libevent loop
  - Filter Chains             - Filter Chains             - Filter Chains
  - Upstream Connection Pool  - Upstream Connection Pool  - Upstream Connection Pool
```

---

## 2. Fundamental Terminology

- **Host**: An entity capable of network communication (IP/port).
- **Downstream**: The client initiating the connection to Envoy.
- **Upstream**: The backend service to which Envoy forwards traffic.
- **Listener**: A named network location (e.g., port 10000) that accepts downstream connections.
- **Cluster**: A group of logically similar upstream hosts that Envoy balances traffic across.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (07-Traefik-Cloud-Native-Reverse-Proxy)](../07-Traefik-Cloud-Native-Reverse-Proxy/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - xDS Dynamic Configuration APIs →](./02-xDS-Dynamic-Configuration-APIs.md) |
