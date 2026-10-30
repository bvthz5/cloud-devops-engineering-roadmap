# 03 - Embedded DNS and Service Discovery

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to Docker's internal embedded DNS server (`127.0.0.11`), automatic service discovery, network aliases, client-side DNS round-robin load balancing, and application caching pitfalls.

---

## 🏛️ 1. What Is Container Service Discovery & Why Are Static IPs an Antipattern?

In static virtual machine or bare-metal server environments, system administrators historically configured services using fixed, static IP addresses recorded in configuration files.

In cloud-native containerized architectures, **static IP assignment is an operational antipattern**. Container IP addresses are fundamentally ephemeral:
- When a container crashes, updates, or scales, Docker destroys the old instance and provisions a new container.
- The new container is dynamically allocated the next available private IP address from the bridge subnet.
- If dependent microservices hardcode IP addresses, every container restart triggers immediate network partition outages across the entire application stack.

**Service discovery** is the automated mechanism that allows containerized applications to locate and connect to sibling services using logical, human-readable hostnames (such as `database`, `redis`, or `auth-api`) rather than dynamic IP addresses. Docker provides native, enterprise-grade service discovery out of the box through its **Embedded DNS Engine**.

---

## ⚙️ 2. How the Embedded DNS Engine (`127.0.0.11`) Works Internally

When a container is attached to any user-defined network (bridge, overlay, or macvlan), Docker automatically creates an isolated `/etc/resolv.conf` file inside the container namespace and injects a single nameserver entry:

```text
nameserver 127.0.0.11
options ndots:0
```

`127.0.0.11` is a special loopback address routed directly to Docker Engine's embedded DNS server daemon running inside the host.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   DOCKER EMBEDDED DNS ARCHITECTURE                     │
└────────────────────────────────────────────────────────────────────────┘

 [ Application Container: "api" ]
   Attempts: curl http://database:5432
   │
   ▼ (Sends UDP DNS query for "database" to 127.0.0.11:53)
 [ Docker Embedded DNS Server (127.0.0.11) ]
   │
   ├─► 1. Query matches internal network registry?
   │      - Is "database" a container name, service name, or alias?
   │      - YES! ──► Returns Container IP: 172.18.0.3 (Zero internet latency)
   │
   └─► 2. External domain query? (e.g. "api.stripe.com")
          - NO internal match!
          - Forwards query to upstream Host / Cloud DNS (e.g. 1.1.1.1, 8.8.8.8)
          - Returns public IP back to container!
```

### Detailed Resolution Process:
1. **Internal Service Queries:** When your application issues a connection request to `database`, the container's standard C library resolver queries `127.0.0.11:53`. The Docker daemon consults its internal, in-memory table of active containers attached to that specific network. If a match is found, Docker returns the container's current private IP address in less than a millisecond.
2. **Upstream External Resolution:** If the queried hostname does not match any local container on the network (for example, when connecting to `api.github.com` or `storage.googleapis.com`), Docker's DNS engine transparently forwards the query to the upstream DNS servers configured on the host machine (or defined via Docker's `--dns` flags).

---

## 🔬 3. Network Aliases and DNS Round-Robin Load Balancing

Docker's embedded DNS engine supports multi-container **Network Aliases**. A network alias allows multiple independent containers on the same custom network to share the exact same DNS hostname.

When an application queries a hostname associated with multiple containers, Docker's embedded DNS server returns the IP addresses of **all active containers**, automatically rotating the list order on each query (**Round-Robin DNS**):

```text
┌────────────────────────────────────────────────────────────────────────┐
│                 DNS ROUND-ROBIN REPLICA LOAD BALANCING                 │
└────────────────────────────────────────────────────────────────────────┘

 Client Query 1 to "search-worker":
 127.0.0.11 returns: [ 172.18.0.5, 172.18.0.6, 172.18.0.7 ] ──► Routes to Replica 1

 Client Query 2 to "search-worker":
 127.0.0.11 returns: [ 172.18.0.6, 172.18.0.7, 172.18.0.5 ] ──► Routes to Replica 2

 Client Query 3 to "search-worker":
 127.0.0.11 returns: [ 172.18.0.7, 172.18.0.5, 172.18.0.6 ] ──► Routes to Replica 3
```

### Deploying Network Aliases via CLI:
```bash
# 1. Create custom isolated network
docker network create internal_mesh

# 2. Spawn three identical background worker containers sharing the 'worker' alias
docker run -d --name worker-1 --net internal_mesh --net-alias worker worker-image
docker run -d --name worker-2 --net internal_mesh --net-alias worker worker-image
docker run -d --name worker-3 --net internal_mesh --net-alias worker worker-image

# 3. Test DNS resolution from a client container
docker run --rm --net internal_mesh alpine nslookup worker
```

---

## ⚠️ 4. Application DNS Caching Pitfalls & JVM Gotchas

While Docker's embedded DNS server responds dynamically to container restarts and scaling events, **client-side application runtimes frequently cache DNS responses indefinitely**, causing service discovery failures.

### 1. The Java Virtual Machine (JVM) Infinite Cache:
By default, older JVM implementations cache successful DNS lookups **forever** (`networkaddress.cache.ttl = -1`). If a database container is recreated and receives a new IP address, a Java application running in another container continues attempting connections to the old, deceased IP forever!
- **Solution:** Configure JVM DNS TTL to a low value (e.g. 5 to 30 seconds) in `$JAVA_HOME/conf/security/java.security`:
  ```properties
  networkaddress.cache.ttl=10
  networkaddress.cache.negative.ttl=5
  ```

### 2. Node.js `keepAlive` HTTP Agents:
In Node.js applications, using HTTP connection pooling (`http.Agent({ keepAlive: true })`) maintains persistent TCP sockets. When a downstream container restarts, Node.js may attempt to reuse dead sockets until connection timeouts occur. Applications should implement healthchecks and graceful socket recreation.

---

## 🛠️ 5. Custom Upstream DNS Configuration

In enterprise corporate environments operating internal DNS servers or split-horizon DNS, containers can be configured to use specific upstream resolvers:

```bash
# Configure custom upstream DNS servers and search domains via CLI
docker run -d \
  --name enterprise_app \
  --dns 10.0.0.2 \
  --dns 10.0.0.3 \
  --dns-search corp.internal \
  my-app-image
```

Or configure globally across all containers on the host in `/etc/docker/daemon.json`:
```json
{
  "dns": ["10.0.0.2", "1.1.1.1"],
  "dns-search": ["corp.internal"]
}
```

---

## 📋 6. Summary and Navigation

Docker's embedded DNS server (`127.0.0.11`) elevates container networking into a dynamic, self-healing service discovery fabric. By allowing services to connect using immutable logical names and supporting round-robin alias load balancing, applications achieve high operational agility across rolling updates and multi-container architectures.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 02 - Bridge Networking and Veth Pairs](./02-Bridge-Networking-and-Veth-Pairs.md) | [Index](../../../README.md) | [04 - Port Publishing vs Exposing and iptables →](./04-Port-Publishing-vs-Exposing-and-iptables.md) |
