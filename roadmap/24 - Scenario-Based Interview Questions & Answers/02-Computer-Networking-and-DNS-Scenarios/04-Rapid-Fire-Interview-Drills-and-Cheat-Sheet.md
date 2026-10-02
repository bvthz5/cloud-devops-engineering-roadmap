# Computer Networking & DNS Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Insecure Cleartext Traffic across Cloud VPC Peering Connections

### 🚨 The Production Scenario
A compliance audit discovers that microservices communicating across VPC peering connections transmit unencrypted HTTP and database traffic in cleartext.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
VPC peering connects software-defined VPCs across private cloud networks, but cloud providers do not automatically encrypt cross-VPC traffic on older instance types or non-Nitro instances. Internal threat actors or compromised nodes in either VPC can sniff raw traffic via promiscuous packet capture.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Audit cross-VPC communication ports and protocols using VPC Flow Logs.
- Step 2: Enforce mandatory TLS on all internal API endpoints (HTTPS / gRPC with mTLS).
- Step 3: Deploy an automated certificate provisioning operator (cert-manager with HashiCorp Vault or private CA).
- Step 4: Implement network encryption at layer 3/4 using WireGuard, IPSec, or AWS Nitro encryption-in-transit.
- Step 5: Validate zero cleartext traffic using deep packet inspection.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Inspect packet payloads to verify if unencrypted ASCII HTTP headers/data are leaking
tcpdump -nn -A -i eth0 'tcp port 8080 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'

# Verify internal service mutual TLS (mTLS) certificate trust chain
openssl s_client -connect 10.100.1.50:8443 -CAfile /etc/ssl/internal-ca.crt

# Verify if cloud VM hardware supports native hardware-accelerated encryption in transit
aws ec2 describe-instance-types --instance-types c5.large --query 'InstanceTypes[*].NetworkInfo.EncryptionInTransitSupported'

# Inspect active WireGuard point-to-point cross-VPC encrypted tunnel status
wg show

# Enforce hard block on unencrypted HTTP traffic across internal interface
iptables -A INPUT -p tcp --dport 80 -j REJECT --reject-with tcp-reset

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "VPC peering does not automatically guarantee zero-trust encryption in transit. To remediate cleartext leaks, I implement service-level Mutual TLS (mTLS) with automated certificate rotation via cert-manager, utilize modern cloud instance types that support native Nitro/Graviton hardware encryption in transit, and enforce security policies (via Calico or AWS Security Groups) rejecting all unencrypted TCP ports."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: Intermittent 5-Second DNS Latency in Kubernetes from ndots:5** | `kubectl exec <POD> -- cat /etc/resolv.conf` | By default, `/etc/resolv.conf` in Kubernetes pods specifies `options ndots:5` an... |
| **Scenario 2: Asymmetric Routing Causing Packet Drops across Cloud Interconnect** | `sysctl net.ipv4.conf.all.rp_filter` | Asymmetric routing occurs when packets take one network path outbound (e.g., Dir... |
| **Scenario 3: HTTP 504 Gateway Timeout between Load Balancer and Backend Pods** | `curl -iv --keepalive-time 60 https://api.mycompany.com/health` | HTTP Keep-Alive idle timeout mismatch. If the backend application or reverse pro... |
| **Scenario 4: TCP SYN Flood and Conntrack Table Exhaustion** | `cat /proc/sys/net/netfilter/nf_conntrack_count` | Stateful firewalls (`iptables`, `nftables`, Linux `netfilter`) maintain a connec... |
| **Scenario 5: Path MTU Discovery (PMTUD) Black Hole over Overlay Networks** | `ping -M do -s 1472 10.0.1.10` | VXLAN, Geneve, and IPSec encapsulation add 50-70 bytes of tunnel header overhead... |
| **Scenario 6: Latency Spikes from TCP Nagle's Algorithm and Delayed ACKs** | `tcpdump -nn -i eth0 -tt 'tcp[tcpflags] & (tcp-push\|tcp-ack) != 0'` | The deadly interaction between Nagle's Algorithm (`TCP_NODELAY` disabled by defa... |
| **Scenario 7: SSL/TLS Handshake Latency Spike and OCSP Stapling Failure** | `openssl s_client -connect myapp.com:443 -status -tlsextdebug < /dev/null \| grep -A 10 'OCSP response'` | When clients establish a TLS connection, their browsers contact the Certificate ... |
| **Scenario 8: WebSocket Connections Terminating Every 60 Seconds through Load Balancer** | `wscat -c wss://api.myapp.com/ws` | WebSockets maintain long-lived stateful TCP connections over standard HTTP ports... |
| **Scenario 9: BGP Route Flapping and Packet Loss across Multi-Cloud Interconnects** | `vtysh -c 'show ip bgp summary'` | BGP route flapping is triggered when keepalive packets (`HoldTime` default 90s, ... |
| **Scenario 10: Insecure Cleartext Traffic across Cloud VPC Peering Connections** | `tcpdump -nn -A -i eth0 'tcp port 8080 and (((ip[2:2] - ((ip[0]&0xf)<<2)) - ((tcp[12]&0xf0)>>2)) != 0)'` | VPC peering connects software-defined VPCs across private cloud networks, but cl... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Computer Networking & DNS Interview Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Git & Version Control Interview Scenarios: Production Incidents & Triage Scenarios →](../03-Git-and-Version-Control-Interview-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

