# Computer Networking & DNS Interview Scenarios: Architecture & Scaling Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 4: TCP SYN Flood and Conntrack Table Exhaustion

### 🚨 The Production Scenario
Under a traffic surge, a cloud gateway starts dropping new incoming connections. `dmesg` reports `nf_conntrack: table full, dropping packet`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Stateful firewalls (`iptables`, `nftables`, Linux `netfilter`) maintain a connection tracking table (`nf_conntrack`). Under high connection churn or SYN flood attacks, every TCP handshake creates a tracking entry. When the table reaches `nf_conntrack_max`, all new connections are discarded regardless of available CPU or memory.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check current conntrack usage via `cat /proc/sys/net/netfilter/nf_conntrack_count`.
- Step 2: Compare against `nf_conntrack_max`.
- Step 3: Increase `nf_conntrack_max` and resize the hash table bucket count (`nf_conntrack_buckets`).
- Step 4: Reduce TCP connection timeout states (`tcp_timeout_established`, `tcp_timeout_close_wait`).
- Step 5: Bypass connection tracking (`NOTRACK` / raw table) for stateless high-throughput reverse proxies.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check total current tracked connections in kernel memory
cat /proc/sys/net/netfilter/nf_conntrack_count

# Display configured maximum conntrack capacity limit
cat /proc/sys/net/netfilter/nf_conntrack_max

# Double conntrack maximum capacity to 1 million entries
sysctl -w net.netfilter.nf_conntrack_max=1048576

# Resize conntrack hash buckets dynamically without reboot
echo 262144 | sudo tee /sys/module/nf_conntrack/parameters/hashsize

# Bypass connection tracking entirely for stateless HTTP port 80 traffic
iptables -t raw -A PREROUTING -p tcp --dport 80 -j NOTRACK

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "The `nf_conntrack: table full` error indicates that Linux netfilter state tracking has reached capacity, which drops all new connections. I triage this by checking `/proc/sys/net/netfilter/nf_conntrack_count`. To remediate, I increase `nf_conntrack_max` and `hashsize`, decrease `tcp_timeout_established` from its 5-day default to 1 hour, and for high-throughput ingress proxies, I implement `-j NOTRACK` in iptables raw table to process connections statelessly."

---

## 📌 Scenario 5: Path MTU Discovery (PMTUD) Black Hole over Overlay Networks

### 🚨 The Production Scenario
SSH connections and ping (small packets) connect instantly across an AWS Transit Gateway or Calico VXLAN tunnel, but `curl` or large HTTPS transfers hang indefinitely.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
VXLAN, Geneve, and IPSec encapsulation add 50-70 bytes of tunnel header overhead, reducing effective MTU from standard 1500 bytes to 1420-1450 bytes. When a sender transmits a 1500-byte packet with the 'Do Not Fragment' (DF) bit set, the intermediate router drops it and sends back an ICMP Type 3 Code 4 ('Fragmentation Needed') packet. If firewalls block ICMP, the sender never receives the notification and retransmits forever — creating a PMTUD black hole.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Test packet size limits using `ping -M do -s <size> <destination>` to find maximum allowable MTU.
- Step 2: Enable ICMP Type 3 Code 4 traffic through all security groups and firewalls.
- Step 3: Enable TCP Maximum Segment Size (MSS) clamping on the tunnel gateways (`iptables -t mangle -A POSTROUTING -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu`).
- Step 4: Adjust MTU on virtual network interfaces (e.g., `eth0` or `tun0`) to match tunnel overhead.
- Step 5: Verify large file transfer completion with `curl -v -o /dev/null https://largefile.internal`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Probe path with 1500-byte packet (1472 payload + 28 IP/ICMP headers) with DF bit set
ping -M do -s 1472 10.0.1.10

# Find the exact reduced MTU boundary where packets pass unfragmented
ping -M do -s 1420 10.0.1.10

# Enforce TCP MSS clamping dynamically on intermediate Linux router
iptables -t mangle -A FORWARD -p tcp --tcp-flags SYN,RST SYN -j TCPMSS --clamp-mss-to-pmtu

# Adjust local interface MTU to match overlay network capacity
ip link set dev eth0 mtu 1420

# Trace path MTU discovery hops and identify where MTU downshifts occur
tracepath 10.0.1.10

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "When small packets like ping connect but large HTTP payloads hang, it is a textbook Path MTU Discovery (PMTUD) black hole. Tunnel encapsulation (VXLAN/IPSec) shrinks the MTU, and dropped ICMP Type 3 Code 4 packets prevent the sender from renegotiating smaller segment sizes. I verify using `ping -M do -s <bytes>` to find the MTU ceiling, allow ICMP fragmentation warnings through security groups, and enforce TCP MSS clamping (`--clamp-mss-to-pmtu`) on routers."

---

## 📌 Scenario 6: Latency Spikes from TCP Nagle's Algorithm and Delayed ACKs

### 🚨 The Production Scenario
A real-time microservices trading API written in Go and Java experiences 40ms to 200ms latency spikes on small JSON/protobuf request payloads under low traffic.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
The deadly interaction between Nagle's Algorithm (`TCP_NODELAY` disabled by default in some runtimes) on the client side and TCP Delayed ACKs on the server side. Nagle holds small outgoing packets until a previous ACK is received or a full MSS buffer is assembled. The receiver delays sending an ACK for up to 40ms-200ms expecting return data. Both sides wait on each other, producing fixed 40ms/200ms latency steps.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Profile packet latency using `tcpdump` and inspect delta between small frames and ACK replies.
- Step 2: Enable `TCP_NODELAY` (disable Nagle's algorithm) on all client and server HTTP/gRPC connection pools.
- Step 3: Enable `TCP_QUICKACK` on Linux server sockets where real-time responses are required.
- Step 4: Use HTTP/2 or gRPC persistent multiplexed streams rather than ad-hoc single HTTP/1.1 connections.
- Step 5: Verify p99 latency drop from 200ms to <2ms on synthetic benchmarks.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Capture timestamped TCP packets to observe exact 40ms/200ms gaps
tcpdump -nn -i eth0 -tt 'tcp[tcpflags] & (tcp-push|tcp-ack) != 0'

# Inspect socket internal options: check if 'nodelay' is active or if 'wscale' is stalling
ss -tin '( sport = :8080 )'

# Prevent TCP congestion window from collapsing to 1 MSS after idle periods
sysctl -w net.ipv4.tcp_slow_start_after_idle=0

# Inspect TCP Segmentation Offload (TSO) hardware offload status
ethtool -k eth0 | grep -i tso

# Benchmark network bandwidth and latency with Nagle disabled (-N flag)
iperf3 -c target.host -N

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "The 40ms/200ms latency cliff is the classic signature of Nagle's algorithm colliding with Delayed ACKs. Nagle buffers small packets waiting for an outstanding ACK, while the server waits 40ms before sending a standalone ACK. I solve this immediately by setting `TCP_NODELAY` on client and server sockets to disable Nagle for latency-sensitive microservices, setting `tcp_slow_start_after_idle=0`, and utilizing persistent gRPC/HTTP/2 multiplexed streams."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Computer Networking & DNS Interview Scenarios: Production Incidents & Triage Scenarios](01-Production-Incidents-and-Triage-Scenarios.md) | [Index](../../../README.md) | [Computer Networking & DNS Interview Scenarios: Security & Disaster Recovery Scenarios →](03-Security-and-Disaster-Recovery-Scenarios.md) |

