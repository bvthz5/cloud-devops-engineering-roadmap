# 02 - Ingress-Nginx Architecture and Controller Mechanics

## 1. How Ingress-Nginx Works

`ingress-nginx` is composed of:
1. **Control Loop (Go daemon):** Watches `Ingress`, `Service`, `Endpoints`, and `Secret` resources in the Kubernetes API.
2. **Data Plane (Nginx + OpenResty Lua):** High-performance reverse proxy that handles incoming client requests.

```text
Kubernetes API ──► Ingress-Nginx Controller (Go)
                        │
                        ▼ (Dynamic Shared Memory / Lua)
              [ Nginx Master Process ]
                        ├── Worker Process 1 (Serving traffic)
                        └── Worker Process 2 (Serving traffic)
```

### Dynamic Lua Routing vs Static Reloads
Older ingress controllers had to rewrite `/etc/nginx/nginx.conf` and issue `nginx -s reload` every time a pod scaled up or down. Under high churn, this caused high CPU and dropped connections.
Modern `ingress-nginx` uses **Lua shared memory dictionaries (`lua_shared_dict`)** to update upstream endpoints dynamically **without reloading Nginx!**

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Ingress Resource Specification and Path Routing](./01-Ingress-Resource-Specification-and-Path-Routing.md) | [Index](../../../README.md) | [03 - SSL TLS Termination and Cert Manager Integration →](./03-SSL-TLS-Termination-and-Cert-Manager-Integration.md) |
