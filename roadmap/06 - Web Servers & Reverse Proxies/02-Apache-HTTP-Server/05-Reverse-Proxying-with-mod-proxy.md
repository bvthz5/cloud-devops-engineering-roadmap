# 05 - Reverse Proxying with mod_proxy

## 1. Reverse Proxying with `mod_proxy` and `mod_proxy_http`

Apache can terminate client connections and proxy requests to backend application servers (Node.js, Python Uvicorn, Java Spring Boot).

```apache
<VirtualHost *:80>
    ServerName api.example.com

    # Pass the original Host header to backend
    ProxyPreserveHost On

    # Forward client IP headers
    ProxyAddHeaders On

    # Route /api/ to backend microservice
    ProxyPass /api/ http://127.0.0.1:8080/api/ retry=1 acquire=3000 timeout=60 Keepalive=On
    ProxyPassReverse /api/ http://127.0.0.1:8080/api/

    # Connection pool tuning
    <Proxy "http://127.0.0.1:8080">
        Require all granted
    </Proxy>
</VirtualHost>
```

---

## 2. Load Balancing with `mod_proxy_balancer`

```apache
<VirtualHost *:80>
    ServerName cluster.example.com

    <Proxy "balancer://appcluster">
        # Node 1
        BalancerMember http://10.0.1.10:8080 loadfactor=3
        # Node 2
        BalancerMember http://10.0.1.11:8080 loadfactor=1
        # Standby Backup Node
        BalancerMember http://10.0.1.12:8080 status=+H

        ProxySet lbmethod=byrequests
    </Proxy>

    ProxyPass / balancer://appcluster/
    ProxyPassReverse / balancer://appcluster/
</VirtualHost>
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - URL Rewriting](./04-URL-Rewriting-with-mod-rewrite.md) | [README](./README.md) | [06 - Security Hardening & Module Management](./06-Security-Hardening-and-Module-Management.md) |
