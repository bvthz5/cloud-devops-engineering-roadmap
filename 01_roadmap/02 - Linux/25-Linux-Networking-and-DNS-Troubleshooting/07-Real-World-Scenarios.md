# 07 — Real-World Production Networking Scenarios

Real-world network issues rarely produce clean error messages. Instead, systems suffer from intermittent latency spikes, connection timeouts, or silent packet drops. Here are four common production outage scenarios and their solutions.

---

## Scenario 1: High `TIME_WAIT` Socket Exhaustion on Reverse Proxies

### Incident Summary
A high-traffic NGINX reverse proxy forwarding requests to a microservice backend suddenly starts rejecting traffic with:
`connect() failed (99: Cannot assign requested address) while connecting to upstream`.

### Root Cause Analysis
- HTTP/1.0 or HTTP/1.1 connections without keep-alive close after every request.
- The proxy initiates active close, placing sockets into `TIME_WAIT` for 60 seconds (2 * MSL).
- The Linux kernel ephemeral port range (`net.ipv4.ip_local_port_range`) is exhausted (default ~28,000 ports). No free ports remain to connect to the backend IP:port tuple.

### Production Solution
1. **Application Fix (Best Practice):** Enable HTTP Keep-Alive in NGINX upstream configuration:
   ```nginx
   upstream backend_pool {
       server 10.0.1.50:8080;
       keepalive 128; # Maintain pool of open persistent idle connections
   }
   server {
       location / {
           proxy_pass http://backend_pool;
           proxy_http_version 1.1;
           proxy_set_header Connection "";
       }
   }
   ```
2. **Kernel Tuning (`/etc/sysctl.d/99-network.conf`):**
   ```ini
   # Allow reuse of TIME_WAIT sockets for outgoing connections
   net.ipv4.tcp_tw_reuse = 1
   # Expand ephemeral port range
   net.ipv4.ip_local_port_range = 10240 65535
   ```
   Apply with: `sudo sysctl --system`

---

## Scenario 2: The MTU Blackhole in Overlay Networks (Kubernetes / VXLAN)

### Incident Summary
Pods communicate normally when sending small payloads (e.g. health checks `curl /healthz`). However, API calls returning large JSON responses (> 1500 bytes) hang indefinitely until timing out.

### Root Cause Analysis
- Host interface MTU is `1500`.
- VXLAN adds a 50-byte encapsulation header, reducing available MTU to `1450`.
- The pod sends a 1500-byte packet with the DF (Don't Fragment) bit set.
- The node tries to encapsulate the packet, sees it exceeds 1500 bytes, and drops it. Intermediate firewalls block the ICMP "Fragmentation Needed" response, creating an **MTU Blackhole**.

### Production Solution
1. Ensure the CNI configuration sets MTU to `1450` (or `1420` for Geneve).
2. Configure TCP MSS Clamping in iptables on nodes:
   ```bash
   sudo iptables -t mangle -A POSTROUTING -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu
   ```

---

## Scenario 3: Kubernetes `ndots:5` DNS Latency Amplification

### Incident Summary
A Node.js microservice calling `api.stripe.com` experiences 20–50ms added latency on every single outbound HTTP request. CoreDNS CPU utilization spikes to 90%.

### Root Cause Analysis
By default, Kubernetes pods have `/etc/resolv.conf` configured with `ndots:5`:
```text
search default.svc.cluster.local svc.cluster.local cluster.local c.project.internal
options ndots:5
```
Because `api.stripe.com` contains only 2 dots (< 5), the resolver first tries:
1. `api.stripe.com.default.svc.cluster.local` -> NXDOMAIN
2. `api.stripe.com.svc.cluster.local` -> NXDOMAIN
3. `api.stripe.com.cluster.local` -> NXDOMAIN
4. `api.stripe.com.c.project.internal` -> NXDOMAIN
5. `api.stripe.com.` -> SUCCESS!

Each outbound call triggers **5 sequential DNS queries**!

### Production Solution
- **Option A:** Append a trailing dot in application URLs: `https://api.stripe.com./v1/charges` (marks domain as absolute FQDN, skipping search path).
- **Option B:** Override pod `dnsConfig` in Kubernetes Deployment:
  ```yaml
  spec:
    dnsConfig:
      options:
        - name: ndots
          value: "2"
  ```
- **Option C:** Deploy `NodeLocal DNSCache` daemonset to cache resolution locally on every node.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Network Namespaces and Container Networking](./06-Network-Namespaces-and-Container-Networking.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
