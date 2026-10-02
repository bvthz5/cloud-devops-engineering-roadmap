# 03 - Embedded DNS and Service Discovery

## 1. The 127.0.0.11 Resolver

On any user-defined bridge network, Docker injects an internal DNS server listening on **`127.0.0.11`** into the container's `/etc/resolv.conf`:

```text
[ App Container (curl http://database:5432) ]
                      │
                      ▼ (DNS Query: "database")
           [ 127.0.0.11:53 (Docker DNS Engine) ]
                      │
        ┌─────────────┴─────────────┐
        ▼                           ▼
(Is "database" a container?)    (External domain: "google.com")
  ├──► YES: Return 172.18.0.3     └──► Forward to Host / Upstream DNS (8.8.8.8)
```

---

## 2. Network Aliases

Multiple containers can share the same **network alias** on a custom bridge, providing simple round-robin client-side load balancing:

```bash
docker network create internal_net

# Spawn two worker containers sharing the alias 'worker'
docker run -d --net internal_net --net-alias worker worker_image
docker run -d --net internal_net --net-alias worker worker_image

# Querying 'worker' alternates between both container IP addresses!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Bridge Networking & Veth Pairs](./02-Bridge-Networking-and-Veth-Pairs.md) | [README](./README.md) | [04 - Port Publishing vs Exposing](./04-Port-Publishing-vs-Exposing-and-iptables.md) |
