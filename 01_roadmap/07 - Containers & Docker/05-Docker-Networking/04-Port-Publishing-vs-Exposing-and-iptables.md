# 04 - Port Publishing vs. Exposing and iptables

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to container port exposure versus publishing, Linux Netfilter packet routing, the `docker-proxy` subsystem, the critical UFW firewall bypass vulnerability, and the `DOCKER-USER` defense chain.

---

## 🏛️ 1. What Is Port Exposure vs. Port Publishing?

In containerized application deployment, a common source of confusion among engineers is the distinction between **exposing** a port and **publishing** a port:

### 1.1 `EXPOSE` (Documentation Contract)
- **What it is:** An instruction written inside a Dockerfile (e.g., `EXPOSE 8080`).
- **Working Mechanism:** `EXPOSE` is **purely informational metadata**. It documents to developers and automated orchestrators which port the container application intends to listen on.
- **Security Reality:** Declaring `EXPOSE` does **not** publish the port to the host operating system, does **not** open any host firewall ports, and does **not** make the container accessible to external clients on the network.

### 1.2 `-p` / `--publish` (Active Network Plumbing)
- **What it is:** A runtime flag passed during `docker run` (or declared under `ports:` in Docker Compose).
- **Working Mechanism:** Actively commands the Docker daemon to bind a physical TCP/UDP port on the host machine and configure Linux kernel routing rules to forward incoming traffic from that host port into the container's private IP and port.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   PORT PUBLISHING ARCHITECTURAL FLOW                   │
└────────────────────────────────────────────────────────────────────────┘

 External Client Browser ──► Requests: http://203.0.113.10:80 (Host Public IP)
                                   │
                                   ▼
 [ Host Linux Network Interface: eth0 ]
                                   │
                                   ▼
 [ Linux Netfilter (iptables PREROUTING Chain) ]
   DNAT Rule: Rewrite Destination IP 203.0.113.10:80 ──► 172.18.0.4:8080
                                   │
                                   ▼
 [ Linux Bridge: br-prod ] ──► [ veth Cable ] ──► [ Container Port: 8080 ]
```

---

## ⚙️ 2. How Docker Manipulates `iptables` and the `docker-proxy`

When you publish a port using `docker run -d -p 80:8080 my-web-app`, Docker performs two complementary operations to guarantee traffic delivery:

### 1. Kernel-Level Destination NAT (DNAT):
Docker inserts a Destination Network Address Translation (DNAT) rule into the Linux kernel's `nat` table under the `PREROUTING` and `DOCKER` chains. When external packets arrive on host port 80, the Linux kernel rewrites the destination IP from the host's public IP to the container's private IP (`172.18.0.4`) and changes the destination port to `8080`.

### 2. The Userland Proxy (`docker-proxy`):
In addition to kernel `iptables` rules, Docker spawns a small Go daemon named `docker-proxy` listening on host port 80. The userland proxy handles connections originating from `localhost` on the host machine where hairpinned kernel NAT routing might fail. In modern Linux systems, this can be disabled via daemon configuration (`"userland-proxy": false`) to save memory if raw kernel routing is preferred.

---

## 🚨 3. The Dangerous "UFW Firewall Bypass" Vulnerability

One of the most dangerous and notorious security gotchas in Linux container administration is that **Docker silently bypasses standard host firewalls**, such as Ubuntu's **Uncomplicated Firewall (UFW)** and Red Hat's **firewalld**.

### Why the Bypass Occurs:
In the Linux kernel's Netfilter architecture, incoming network packets traverse firewall chains in a strict, unchangeable hierarchy:
1. `PREROUTING` Chain (Evaluated First — Where Docker inserts its DNAT rules)
2. Routing Decision
3. `FORWARD` Chain (Where Docker forwards traffic to containers)
4. `INPUT` Chain (Where UFW places its block and allow rules!)

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   THE UFW FIREWALL BYPASS MECHANISM                    │
└────────────────────────────────────────────────────────────────────────┘

 Inbound Packet from Public Internet (Port 6379 - Redis)
         │
         ▼
 [ 1. PREROUTING Chain ] ──► Docker DNAT redirects packet to Container IP!
         │
         ▼
 [ 2. Routing Decision ] ──► Packet is marked for FORWARDING, not host INPUT!
         │
         ├─────────────────────────────────────────┐
         ▼                                         ▼
 [ 3. FORWARD Chain ]                      [ 4. INPUT Chain ]
 (Bypasses UFW! Packet sent to container!)  (Where UFW "deny" rules live!
                                             Never reached by packet!)
```

### The Catastrophic Result:
A system administrator executes `ufw default deny incoming` and enables the firewall, believing the server is completely secured. Later, an engineer runs a Redis container with `docker run -d -p 6379:6379 redis`. 

Because Docker's rules operate in the `PREROUTING` and `FORWARD` chains, **the Redis database is instantly published directly to the public internet**, completely bypassing UFW without warning!

---

## 🛡️ 4. The Enterprise Solution: The `DOCKER-USER` Chain

To allow administrators to filter external traffic before Docker handles it, Docker explicitly reserves a custom iptables chain named **`DOCKER-USER`**.

The `DOCKER-USER` chain is evaluated at the very beginning of the `FORWARD` filtering process, before any of Docker's internal routing rules execute. Any rule placed in `DOCKER-USER` takes absolute precedence:

```bash
# 1. Allow established and related connections (so containers can respond)
sudo iptables -I DOCKER-USER -m conntrack --ctstate RELATED,ESTABLISHED -j ACCEPT

# 2. Allow access to published Docker containers ONLY from a trusted management IP
sudo iptables -A DOCKER-USER -i eth0 -s 198.51.100.25 -j ACCEPT

# 3. Block all other external public internet traffic to all published Docker ports!
sudo iptables -A DOCKER-USER -i eth0 -j DROP
```

---

## 🔒 5. Binding to Specific Host IP Addresses (Localhost Isolation)

The most effective, foolproof defense against accidental public port exposure is **binding published ports explicitly to the host's loopback interface (`127.0.0.1`)** or an internal management IP address:

```bash
# BAD (Binds to 0.0.0.0: Publicly accessible across all host network adapters!)
docker run -d -p 5432:5432 postgres:16-alpine

# SECURE (Binds exclusively to 127.0.0.1: Accessible ONLY from local host processes!)
docker run -d -p 127.0.0.1:5432:5432 postgres:16-alpine

# PRIVATE VPN ONLY (Binds exclusively to host's internal wireguard interface)
docker run -d -p 10.8.0.1:5432:5432 postgres:16-alpine
```

In Docker Compose, specify the binding IP directly in the `ports:` directive:
```yaml
services:
  database:
    image: postgres:16-alpine
    ports:
      - "127.0.0.1:5432:5432"    # Safe from external network exposure!
```

---

## 📋 6. Summary and Navigation

Port publishing is a core capability of container networking, but understanding its underlying iptables translation and firewall interactions is essential for enterprise security. By binding sensitive services strictly to localhost and establishing protective filters in the `DOCKER-USER` chain, DevOps engineers safeguard production databases and internal APIs from unauthorized public exposure.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Embedded DNS and Service Discovery](./03-Embedded-DNS-and-Service-Discovery.md) | [Index](../../../README.md) | [05 - Host None and Macvlan Networking →](./05-Host-None-and-Macvlan-Networking.md) |
