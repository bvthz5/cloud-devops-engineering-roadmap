# 03 - Load Balancing Algorithms

## 1. Primary Load Balancing Algorithms

```text
1. Round Robin (Default):
   Requests distributed sequentially across backends (A -> B -> C -> A).

2. Least Connections (least_conn):
   Routes request to the backend with the fewest active TCP connections.
   Best for long-lived transactions (WebSocket, database queries).

3. IP Hash (ip_hash):
   Hashes client IPv4 address to determine backend. Guarantees session persistence
   (sticky sessions) without cookies.

4. Generic Consistent Hashing (hash $key consistent):
   Hashes arbitrary key (e.g., $request_uri or $http_authorization) across a
   consistent Ketama hash ring. Adding/removing a node only remaps a fraction of keys!
```

---

## 2. Nginx Upstream Configuration Examples

```nginx
upstream app_backend {
    # 1. Least Connections
    least_conn;

    # Backend pool with weights
    server 10.0.1.10:8080 weight=3; # Receives 60% of traffic
    server 10.0.1.11:8080 weight=2; # Receives 40% of traffic
    
    # Standby backup server (only used if all primary nodes fail)
    server 10.0.1.99:8080 backup;
    
    # Manually marked unavailable server
    server 10.0.1.12:8080 down;
}

upstream cache_sharded_backend {
    # 2. Consistent Hashing on Request URI (Ketama Ring)
    hash $request_uri consistent;

    server 10.0.2.10:8080;
    server 10.0.2.11:8080;
    server 10.0.2.12:8080;
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Layer 4 vs Layer 7 Load Balancing](./02-Layer-4-vs-Layer-7-Load-Balancing.md) | [README](./README.md) | [04 - Upstream Management & Buffer Tuning](./04-Upstream-Management-and-Buffer-Tuning.md) |
