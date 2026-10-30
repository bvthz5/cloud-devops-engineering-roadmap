# 05 - Host, None, and Macvlan Networking

> **Summary:** A comprehensive, sentence-by-sentence architectural guide to specialized container network modes: high-throughput `--network host`, air-gapped `--network none`, physical LAN integration via `macvlan`, VLAN tagging, and the Macvlan host isolation gotcha.

---

## 🏛️ 1. Why Are Specialized Network Drivers Needed?

While virtual bridge networks satisfy the requirements of 90% of containerized applications, certain specialized enterprise use cases cannot tolerate virtual bridges or NAT translation:
1. **Ultra-Low Latency & High-Throughput Workloads:** Applications processing hundreds of thousands of network packets per second (such as live video streaming, real-time gaming backends, algorithmic trading systems, or load balancers) suffer performance bottlenecks from veth traversal and iptables NAT inspection.
2. **Air-Gapped & High-Security Processing:** Batch tasks processing sensitive customer records, generating cryptographic key pairs, or running untrusted code must be physically barred from communicating over the network.
3. **Legacy Enterprise Infrastructure:** Enterprise networks often enforce static IP tracking, strict switch port security, or physical hardware firewalls that cannot route through a Docker host NAT gateway.

To address these requirements, Docker provides specialized network drivers: **`host`**, **`none`**, **`macvlan`**, and **`ipvlan`**.

---

## ⚙️ 2. Host Networking (`--network host`)

In host networking mode, Docker completely skips the creation of an isolated Linux network namespace for the container.

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   HOST NETWORKING ARCHITECTURAL MODEL                  │
└────────────────────────────────────────────────────────────────────────┘

 [ Host Physical Server ]
   Host Physical IP: 192.168.1.100
   Host Network Stack: eth0, lo, routing table, port space (1 - 65535)
   │
   ├─► Host Process: SSH Daemon (listening on port 22)
   │
   └─► Container: Nginx Web Server (--network host)
       - Runs directly in host network namespace!
       - Binds port 80 directly on physical eth0!
       - ZERO veth cables! ZERO bridges! ZERO iptables NAT overhead!
```

### Key Working Principles:
- **Bare-Metal Line Rate Performance:** Packets move directly between the physical network interface and the container application with zero encapsulation or address translation delays.
- **Port Conflict Rules:** The container shares the host's port namespace. If two containers configured with `--network host` attempt to bind to port 80, the second container crashes with `bind: address already in use`.
- **Security Implications:** Because the container shares the host network namespace, a compromised container process can sniff unencrypted traffic traversing the host and connect directly to services bound to the host's `127.0.0.1` (such as local database sockets).

---

## 🔒 3. None Networking (`--network none`)

The `none` driver provides the most extreme form of network isolation possible in container computing.

### How It Operates Internally:
When a container starts with `--network none`, Docker provisions an isolated Linux network namespace containing **only the local loopback adapter (`lo`, 127.0.0.1)**. Docker allocates no virtual Ethernet cables, assigns no IP address, and establishes no default gateway routes.

```bash
# Run a secure offline cryptographic key generation job
docker run --rm --network none \
  -v ./certs:/output \
  alpine sh -c "openssl genrsa -out /output/private.key 4096"
```

### Operational Use Cases:
1. **Cryptographic Key Generation:** Creating root Certificate Authority (CA) private keys or blockchain signing credentials where network exposure presents an unacceptable risk.
2. **Untrusted Code Execution (Sandboxing):** Executing untrusted student code or automated user submissions without risk of external network attacks or data exfiltration.
3. **Deterministic Batch Data Processing:** Running CPU-intensive machine learning training or data cleansing tasks on local files.

---

## 🌐 4. Macvlan Networking (Physical LAN Integration)

The **`macvlan`** driver allows container virtual interfaces to connect directly to the physical network card attached to the host machine.

Instead of hiding behind the host's IP address through NAT, each container receives its own distinct **hardware MAC address** and an individual **routable IP address** directly from the local physical network subnet:

```text
┌────────────────────────────────────────────────────────────────────────┐
│                   MACVLAN NETWORK ARCHITECTURE                         │
└────────────────────────────────────────────────────────────────────────┘

 Physical Corporate Switch / Router (Subnet: 192.168.1.0/24)
   │
   ▼
 Physical Host NIC: eth0
   │
   ├─► Host Machine IP: 192.168.1.100 (MAC: aa:bb:cc:11:22:33)
   │
   ├─► Container 1:    192.168.1.101 (MAC: 02:42:c0:a8:01:65) ──► Looks like real PC!
   │
   └─► Container 2:    192.168.1.102 (MAC: 02:42:c0:a8:01:66) ──► Looks like real PC!
```

### Creating and Using a Macvlan Network:
```bash
# 1. Create a Macvlan network attached to host physical interface 'eth0'
docker network create -d macvlan \
  --subnet=192.168.1.0/24 \
  --gateway=192.168.1.1 \
  --ip-range=192.168.1.192/27 \
  -o parent=eth0 \
  corp_lan

# 2. Launch container with dedicated physical LAN IP
docker run -d \
  --name legacy_telecom_app \
  --net corp_lan \
  --ip 192.168.1.200 \
  mycompany/telecom:v2.1
```

---

## ⚠️ 5. The Critical Macvlan Host Communication Gotcha

A critical architectural constraint of Linux `macvlan` interfaces is that **by default, the host machine cannot communicate directly with containers running on its own macvlan interface**, and macvlan containers cannot ping the host IP!

### Why This Happens:
For security and loop-prevention reasons, the Linux kernel explicitly forbids traffic originating from a child macvlan sub-interface from looping back into the parent physical network card (`eth0`).

### The Host Macvlan Bridge Workaround:
If the host machine must communicate with its own macvlan containers, an administrator must create a dedicated macvlan sub-interface on the host itself:
```bash
# 1. Create host macvlan link
sudo ip link add macvlan-shim link eth0 type macvlan mode bridge

# 2. Assign an unused IP address from the subnet to the shim interface
sudo ip addr add 192.168.1.250/32 dev macvlan-shim

# 3. Bring the interface up and route container traffic through it
sudo ip link set macvlan-shim up
sudo ip route add 192.168.1.192/27 dev macvlan-shim
```

---

## 📊 6. Summary and Navigation

Specialized Docker network drivers grant engineers fine-grained control over network isolation, routing topology, and hardware utilization. By leveraging `--network host` for raw network line rates, `--network none` for absolute security air-gapping, and `macvlan` for legacy physical subnet integration, infrastructure teams optimize communication architectures for any enterprise constraint.

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Port Publishing vs Exposing and iptables](./04-Port-Publishing-vs-Exposing-and-iptables.md) | [Index](../../../README.md) | [06 - Overlay Networks and Multi Host VXLAN →](./06-Overlay-Networks-and-Multi-Host-VXLAN.md) |
