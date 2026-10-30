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

## 🔌 Production & Cloud DevOps Port Numbers Rapid-Fire Recall Drill

Every Cloud & DevOps engineer must know these standard ports by memory during whiteboard architecture sessions, security group configuration, and incident triage:

| Port | Protocol | Default Service / Component | Common Production / Interview Gotcha |
| :---: | :---: | :--- | :--- |
| **22** | TCP | **SSH / SFTP / SCP** | Security groups should NEVER allow 0.0.0.0/0 on port 22; use AWS SSM Session Manager or Teleport instead. |
| **53** | UDP / TCP | **DNS** | UDP for lookups under 512 bytes; TCP for DNSSEC or zone transfers. Kubernetes CoreDNS listens on 53. |
| **80** | TCP | **HTTP (Cleartext)** | Production ALBs/NGINX should automatically return HTTP 301 redirecting to port 443 HTTPS. |
| **123** | UDP | **NTP (Time Synchronization)** | Unsynchronized clocks break AWS SigV4 request signatures, TLS certificate validation, and distributed DB consensus. |
| **179** | TCP | **BGP (Border Gateway Protocol)** | Used by AWS Direct Connect, Azure ExpressRoute, and Kubernetes CNI (Calico BGP peer mesh). |
| **443** | TCP / UDP | **HTTPS / HTTP/3 (QUIC)** | Standard TLS encrypted web traffic (TCP 443). HTTP/3 and QUIC utilize **UDP port 443**! |
| **1433** | TCP | **Microsoft SQL Server (MSSQL)** | Default target port for Azure SQL / RDS SQL Server security group rules. |
| **1521** | TCP | **Oracle Database** | Default listener port for enterprise Oracle DB instances. |
| **2181** | TCP | **Apache ZooKeeper** | Kafka metadata coordination port (replaced by KRaft mode on 9093 in modern Kafka). |
| **2379** | TCP | **etcd Client** | Kube-apiserver connecting to etcd. Must NEVER be exposed outside the master control plane nodes. |
| **2380** | TCP | **etcd Server-to-Server** | Peer communication port between etcd quorum cluster members. |
| **3000** | TCP | **Grafana Dashboard** | Default Grafana web UI. Often proxied behind NGINX or ALB with TLS on port 443. |
| **3100** | TCP | **Grafana Loki** | Log ingestion and query HTTP API for Promtail / Fluent Bit shippers. |
| **3306** | TCP | **MySQL / MariaDB** | Default database listener. Application pods should connect via private subnet or Cloud SQL proxy. |
| **3389** | TCP / UDP | **RDP (Remote Desktop)** | Windows remote management. Must be shielded behind VPN or Azure Bastion. |
| **4222** | TCP | **NATS Messaging** | Core client protocol port for high-performance cloud-native messaging. |
| **4317** | TCP | **OpenTelemetry Collector (gRPC)** | OTLP telemetry ingestion via gRPC (preferred for high-throughput traces/metrics). |
| **4318** | TCP | **OpenTelemetry Collector (HTTP)** | OTLP telemetry ingestion via HTTP/JSON. |
| **5044** | TCP | **Logstash Beats** | Default ingestion port for Filebeat, Metricbeat, and Packetbeat shippers. |
| **5432** | TCP | **PostgreSQL** | Standard relational database port. In high scale, apps connect through PgBouncer on 6432. |
| **5672** | TCP | **RabbitMQ AMQP** | Advanced Message Queuing Protocol client connection port. |
| **6379** | TCP | **Redis** | In-memory cache and session store. Default binds without auth; must enforce `requirepass` and private VPC. |
| **6432** | TCP | **PgBouncer** | PostgreSQL connection pooler port, shielding database from process exhaustion. |
| **6443** | TCP | **Kubernetes API Server** | Primary secure control plane endpoint (`kube-apiserver`). `kubectl` directs all API calls here. |
| **8080** | TCP | **HTTP Alternate / Jenkins** | Standard Jenkins controller UI or unprivileged user space container web server. |
| **8081** | TCP | **Nexus / SonarQube** | Artifact repository management and static code analysis server interfaces. |
| **8200** | TCP | **HashiCorp Vault API** | Vault client secret management API. Clustered Raft replication operates on **8201**. |
| **8500** | TCP | **HashiCorp Consul HTTP** | Consul service mesh catalog, health checking, and key-value store HTTP API. |
| **9042** | TCP | **Apache Cassandra (CQL)** | Native binary transport for Cassandra Query Language database queries. |
| **9090** | TCP | **Prometheus Server** | Prometheus time-series metrics server web UI and PromQL HTTP query API. |
| **9092** | TCP | **Apache Kafka (Plaintext)** | Default internal broker port. Production external connections mandate TLS/SASL on **9093 / 9094**. |
| **9093** | TCP | **Alertmanager** | Prometheus alert deduplication and notification dispatch engine. |
| **9100** | TCP | **Prometheus Node Exporter** | Host Linux kernel, CPU, memory, and disk metrics scraper endpoint. |
| **9115** | TCP | **Prometheus Blackbox Exporter** | Synthetics and probing endpoint for HTTP/HTTPS, DNS, and ICMP connectivity. |
| **9200** | TCP | **Elasticsearch REST API** | JSON query and ingestion port. Node-to-node cluster transport uses **9300**. |
| **10250** | TCP | **Kubelet API** | Node-level daemon allowing control plane to read logs, fetch metrics, and run `kubectl exec`. |
| **14268** | TCP | **Jaeger Collector HTTP** | Direct span ingestion port for OpenTracing / Jaeger distributed traces. |
| **15001 / 15006** | TCP | **Envoy Outbound / Inbound** | Istio sidecar proxy ports capturing all outbound (15001) and inbound (15006) pod traffic. |
| **15021** | TCP | **Envoy Health Check** | Istio sidecar health check probe port used by kubelet liveness/readiness probes. |
| **15090** | TCP | **Envoy Prometheus Metrics** | Envoy proxy scrape port exposing raw service mesh telemetry to Prometheus. |
| **15672** | TCP | **RabbitMQ Management** | Web UI dashboard and HTTP management API. |
| **16686** | TCP | **Jaeger Query UI** | Distributed tracing search and waterfall visualization interface. |
| **27017** | TCP | **MongoDB** | Standard NoSQL database listener. Config server on **27019**; shard router on **27018**. |
| **30000–32767** | TCP / UDP | **Kubernetes NodePort Range** | Static port allocation on every cluster node for external ingress traffic. |
| **50000** | TCP | **Jenkins Agent (JNLP)** | Inbound connection port for ephemeral build agents to communicate with master. |
| **51820** | UDP | **WireGuard VPN** | High-performance modern kernel-level encrypted VPN tunnel. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Computer Networking & DNS Interview Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Git & Version Control Interview Scenarios: Production Incidents & Triage Scenarios →](../03-Git-and-Version-Control-Interview-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

