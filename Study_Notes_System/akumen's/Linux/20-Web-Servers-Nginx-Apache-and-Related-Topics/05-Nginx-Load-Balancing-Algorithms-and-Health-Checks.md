# 5. Nginx Load Balancing & Health Checks

## Upstream Load Balancing Architecture

Nginx distributes incoming web traffic across pools of backend servers using the `upstream` directive.

```nginx
upstream backend_app {
    # Default: Round-Robin
    server app1.internal:8080 weight=3 max_fails=3 fail_timeout=30s;
    server app2.internal:8080 weight=1 max_fails=3 fail_timeout=30s;
    server app3.internal:8080 backup;
}

server {
    listen 80;
    server_name app.example.com;

    location / {
        proxy_pass http://backend_app;
    }
}
```

## Load Balancing Algorithms

1. **Round-Robin (Default):** Distributes requests sequentially across servers in list.
2. **Weighted Round-Robin:** Directs proportional traffic based on `weight` parameter (e.g., `weight=3` gets 3x requests).
3. **Least Connections (`least_conn;`):** Directs new request to server with fewest active connections. Ideal for long-running transactions.
4. **IP Hash (`ip_hash;`):** Uses client IPv4/IPv6 address hash to maintain session stickiness to same backend server.
5. **Generic Hash (`hash $request_uri consistent;`):** Hashes arbitrary key (URI, header, cookie) for custom routing.

## Passive Health Checks
- `max_fails=N`: Number of failed communication attempts before marking server unavailable.
- `fail_timeout=30s`: Time period server is considered offline after reaching `max_fails`.
- `backup`: Specifies fallback server used only when all primary servers are down.
- `down`: Marks server permanently offline for maintenance.
