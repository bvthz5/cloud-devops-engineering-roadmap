# 09 — Linux Networking Interview Q&A

Real-world technical interview questions asked for DevOps, SRE, and Cloud Infrastructure roles.

---

### Q1: What happens from a networking perspective when you run `curl https://api.example.com` on a Linux server?
**Answer:**
1. **Name Resolution:**
   - Kernel checks `/etc/nsswitch.conf` to determine resolver order.
   - Checks local static mappings in `/etc/hosts`.
   - Queries DNS resolver (e.g. `127.0.0.53` via `systemd-resolved` or upstream nameserver in `/etc/resolv.conf`).
   - Resolver returns the IP address (e.g. `93.184.216.34`).
2. **Route & Interface Selection:**
   - Kernel evaluates routing table (`ip route get 93.184.216.34`).
   - Identifies egress interface (`eth0`) and next-hop gateway.
3. **ARP / Neighbor Lookup:**
   - Checks ARP table (`ip neigh`) for gateway IP. If not found, broadcasts ARP request to learn gateway MAC address.
4. **TCP 3-Way Handshake:**
   - Client sends `SYN` with initial sequence number and MSS.
   - Server responds with `SYN-ACK`.
   - Client sends `ACK` (Connection enters `ESTABLISHED`).
5. **TLS Handshake (HTTPS):**
   - Client sends `ClientHello` (TLS version, cipher suites, SNI header).
   - Server responds with `ServerHello`, certificate chain, and key exchange.
   - Mutual cryptographic keys established.
6. **HTTP Request & Response:**
   - Client sends encrypted HTTP GET request.
   - Server processes and streams HTTP response back over TCP.
7. **Connection Teardown:**
   - 4-way FIN/ACK teardown.

---

### Q2: What is the difference between `TIME_WAIT` and `CLOSE_WAIT` TCP states?
**Answer:**
- **`TIME_WAIT` (Active Close):**
  - The local endpoint initiated the connection shutdown by sending the first `FIN`.
  - The socket remains in `TIME_WAIT` for 2 * MSL (Maximum Segment Lifetime, typically 60s in Linux).
  - Purpose: Ensures delayed packets in transit do not collide with new connections using the same IP:port tuple, and ensures the remote peer receives the final `ACK`.
- **`CLOSE_WAIT` (Passive Close):**
  - The remote peer sent a `FIN` and the local kernel acknowledged it.
  - The socket is now waiting for the **local application process** to call `close()` on the file descriptor.
  - If a system has thousands of `CLOSE_WAIT` sockets, it indicates an **application bug** (resource leak), not a kernel configuration issue.

---

### Q3: Why is `ss` preferred over `netstat` on modern production systems?
**Answer:**
`netstat` reads and parses `/proc/net/tcp` line by line as a text file. On high-scale servers with 50,000+ connections, reading this procfs interface causes significant CPU lockups and high system latency. In contrast, `ss` communicates directly with kernel memory via the `sock_diag` Netlink binary socket interface, executing orders of magnitude faster with negligible overhead.

---

### Q4: What does a high `Recv-Q` mean on a socket in `LISTEN` state?
**Answer:**
On a listening TCP socket, `Send-Q` represents the maximum listen backlog limit (e.g. 128 or 511). `Recv-Q` represents the number of completed 3-way handshakes queued up in the kernel accept queue waiting for the application process to call `accept()`.
If `Recv-Q` is elevated, the application is CPU-saturated, deadlocked, or handling requests too slowly, causing new connections to back up in the kernel queue.

---

### Q5: How do Linux Network Namespaces provide isolation for Docker and Kubernetes containers?
**Answer:**
Network namespaces partition the kernel network subsystem into isolated domains. Each namespace has its own independent loopback interface, network devices, routing tables, firewall rules (iptables/nftables), and socket tables. By placing a container's processes inside a dedicated network namespace and connecting it to the host via a `veth` (virtual ethernet) pair, containers achieve complete network virtualization with native kernel performance.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
