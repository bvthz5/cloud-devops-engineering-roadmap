# 05 — Network Sockets and the BSD Socket API

In Linux, "Everything is a file," including network connections. A **Socket** is an endpoint for communication identified by a file descriptor.

---

## 1. The Server Socket Lifecycle

```text
socket()   ──► Creates an unbound socket descriptor
   │
   ▼
bind()     ──► Binds socket to local IP and Port (e.g. 0.0.0.0:8080)
   │
   ▼
listen()   ──► Marks socket as passive (ready to accept incoming connections)
   │
   ▼
accept()   ──► Blocks until a client completes 3-way handshake; returns NEW socket FD!
   │
   ▼
read() / write() ──► Transfers application payload
   │
   ▼
close()    ──► Closes file descriptor and initiates TCP teardown
```

---

## 2. High-Concurrency Event Notification: `epoll`

In legacy servers, checking 10,000 sockets required scanning an array using `select()` ($O(N)$).
Linux **`epoll`** operates in **$O(1)$** time by having the kernel wake up worker threads only when a socket's state changes. Powers NGINX, Node.js, Envoy, and Redis.

---

## 3. Master Port Numbers & Protocols Reference Guide

A **Port Number** is a 16-bit unsigned integer ($0 \text{ to } 65535$) identifying a specific communication endpoint on an operating system. Port numbers are divided into three standard IANA ranges:

| Port Range | Category Name | Description & Privileges |
| :--- | :--- | :--- |
| **0 – 1023** | **Well-Known / System Ports** | Reserved for core operating system services. Requires `root` or `CAP_NET_BIND_SERVICE` capability to bind on Linux. |
| **1024 – 49151** | **Registered / User Ports** | Assigned by IANA for specific applications, databases, and DevOps services. Can be bound by unprivileged users. |
| **49152 – 65535** | **Dynamic / Private / Ephemeral Ports** | Allocated dynamically by the operating system kernel for outbound client-side connections (e.g., browser or curl source port). |

---

### 🌐 Core Internet & Network Protocols

| Port | Protocol | Service Name | Typical DevOps / Production Usage |
| :---: | :---: | :--- | :--- |
| **20** | TCP | **FTP Data** | File Transfer Protocol data stream |
| **21** | TCP | **FTP Control** | File Transfer Protocol command/control channel |
| **22** | TCP | **SSH / SFTP / SCP** | Secure Shell remote administration and secure file transfers |
| **23** | TCP | **Telnet** | Legacy unencrypted terminal access (strictly avoided in production) |
| **25** | TCP | **SMTP** | Simple Mail Transfer Protocol server-to-server mail relay |
| **53** | UDP / TCP | **DNS** | Domain Name System (UDP for queries <512 bytes, TCP for zone transfers/large responses) |
| **67** | UDP | **DHCP Server** | Dynamic Host Configuration Protocol server listening port |
| **68** | UDP | **DHCP Client** | Dynamic Host Configuration Protocol client listening port |
| **69** | UDP | **TFTP** | Trivial File Transfer Protocol used in PXE network booting |
| **80** | TCP | **HTTP** | Hypertext Transfer Protocol (unencrypted cleartext web traffic) |
| **88** | TCP / UDP | **Kerberos** | Network authentication protocol in Active Directory and Linux realms |
| **110** | TCP | **POP3** | Post Office Protocol version 3 for email retrieval |
| **123** | UDP | **NTP** | Network Time Protocol for clock synchronization across servers |
| **143** | TCP | **IMAP** | Internet Message Access Protocol for email sync |
| **161** | UDP | **SNMP Agent** | Simple Network Management Protocol metric polling |
| **162** | UDP | **SNMP Trap** | Asynchronous network management event alerts |
| **179** | TCP | **BGP** | Border Gateway Protocol used in cloud interconnects and Calico/Cilium CNI |
| **389** | TCP / UDP | **LDAP** | Lightweight Directory Access Protocol for directory user lookup |
| **443** | TCP / UDP | **HTTPS / QUIC** | HTTP over TLS/SSL encryption; UDP 443 powers HTTP/3 (QUIC) |
| **445** | TCP | **SMB / CIFS** | Microsoft Server Message Block file and printer sharing |
| **465** | TCP | **SMTPS** | SMTP over SSL/TLS for secure mail submission |
| **514** | UDP / TCP | **Syslog** | Centralized Linux system logging daemon listener |
| **587** | TCP | **SMTP Submission** | Modern mail client submission port with STARTTLS encryption |
| **636** | TCP | **LDAPS** | Secure LDAP over TLS/SSL |
| **873** | TCP | **rsync** | Remote file and directory synchronization daemon |
| **993** | TCP | **IMAPS** | Secure IMAP over TLS/SSL |
| **995** | TCP | **POP3S** | Secure POP3 over TLS/SSL |

---

---

### 🗄️ Relational, NoSQL & Distributed Database Ports

#### 🐬 MySQL & MariaDB Ecosystem Ports (Exhaustive)
| Port | Protocol | Component / Service | Architecture Function |
| :---: | :---: | :--- | :--- |
| **3306** | TCP | **MySQL Classic SQL Protocol** | Default relational SQL connection port used by applications, ORMs, and `mysql` CLI. |
| **33060** | TCP | **MySQL X Protocol** | X DevAPI, MySQL Document Store (NoSQL JSON collections), and asynchronous pipeline queries. |
| **33061** | TCP | **MySQL Group Replication (MGR)** | Communication and Paxos consensus between members of an InnoDB Cluster / MGR group. |
| **33062** | TCP | **MySQL Administrative Network Interface** | Dedicated `admin_port` for DBAs to connect when `max_connections` is exhausted on 3306! |
| **6032** | TCP | **ProxySQL Admin Interface** | Administrative interface used to configure connection pools, query rules, and users. |
| **6033** | TCP | **ProxySQL Client Traffic** | High-performance client proxy port providing read/write splitting and connection multiplexing. |
| **4567** | TCP / UDP | **Galera Cluster Replication** | wsrep replication traffic, group communication, and multicast/unicast replication. |
| **4568** | TCP | **Galera IST (Incremental State Transfer)** | Fast incremental cache transfer to rejoin a temporarily disconnected node. |
| **4444** | TCP | **Galera SST (State Snapshot Transfer)** | Full binary snapshot sync (via `mariabackup` or `rsync`) when a new node joins the cluster. |
| **1186** | TCP | **MySQL NDB Management Server** | `ndb_mgmd` cluster management daemon communication port. |
| **2202** | TCP | **MySQL NDB Data Node** | Internal communication between NDB storage engine data nodes. |
| **15306** | TCP | **Vitess VTGate (MySQL Protocol)** | Distributed sharded MySQL query routing gateway emulating native MySQL protocol. |
| **15000** | TCP | **Vitess VTGate / VTTablet HTTP** | Health check, status dashboard, and administrative HTTP inspection interface. |

#### 🐘 PostgreSQL & Distributed SQL Ports
| Port | Protocol | Component / Service | Architecture Function |
| :---: | :---: | :--- | :--- |
| **5432** | TCP | **PostgreSQL Client Protocol** | Standard relational database port used by `psql` and client connection pools. |
| **5433** | TCP | **PostgreSQL Secondary / Citus** | Alternative port for parallel instances or Citus distributed coordinator nodes. |
| **6432** | TCP | **PgBouncer Connection Pooler** | Lightweight session, transaction, and statement connection pooler for Postgres. |
| **8008** | TCP | **Patroni REST API** | High-availability leader elections, DCS failover heartbeats, and health probe checks. |
| **8432** | TCP | **Odyssey Connection Pooler** | Scalable multi-threaded connection pooler developed for high-concurrency Postgres. |
| **26257** | TCP | **CockroachDB SQL & Gossip** | Unified SQL client protocol and inter-node Raft consensus port. |
| **5433** | TCP | **YugabyteDB YSQL** | PostgreSQL-compatible distributed SQL client interface. |
| **9042** | TCP | **YugabyteDB YCQL** | Cassandra-compatible distributed NoSQL client interface. |

#### ⚡ Redis, In-Memory Caches & Key-Value Stores
| Port | Protocol | Component / Service | Architecture Function |
| :---: | :---: | :--- | :--- |
| **6379** | TCP | **Redis Server** | Default client connection port for in-memory caching and data structures. |
| **16379** | TCP | **Redis Cluster Bus** | Node-to-node cluster communication: gossip protocol, failover, and slot migration (`port + 10000`). |
| **26379** | TCP | **Redis Sentinel** | High-availability monitoring, health checks, and automated master failover arbitration. |
| **11211** | TCP / UDP | **Memcached** | High-performance distributed memory object caching system. |

#### 🍃 MongoDB Ecosystem Ports
| Port | Protocol | Component / Service | Architecture Function |
| :---: | :---: | :--- | :--- |
| **27017** | TCP | **MongoDB mongod / mongos** | Standard client connection port for MongoDB databases and sharding routers. |
| **27018** | TCP | **MongoDB Shard Instance** | Default listener when running `mongod` as a designated cluster shard member. |
| **27019** | TCP | **MongoDB Config Server** | Metadata and chunk distribution storage for sharded cluster configurations. |
| **28017** | TCP | **MongoDB Legacy HTTP** | Deprecated web-based diagnostic status console. |

#### 📊 Columnar, Time-Series & Search Engine Ports
| Port | Protocol | Component / Service | Architecture Function |
| :---: | :---: | :--- | :--- |
| **1433** | TCP | **Microsoft SQL Server (MSSQL)** | Default relational database listener for Windows/Linux SQL Server. |
| **1521** | TCP | **Oracle Database Listener** | Traditional enterprise Oracle SQL*Net / TNS listener. |
| **7000** | TCP | **Apache Cassandra Inter-Node** | Unencrypted internode storage cluster gossip communication. |
| **7001** | TCP | **Apache Cassandra TLS Inter-Node**| SSL/TLS encrypted internode cluster gossip communication. |
| **7199** | TCP | **Apache Cassandra JMX** | Java Management Extensions port for nodetool and metrics collection. |
| **8086** | TCP | **InfluxDB HTTP API** | Ingestion and Flux query endpoint for time-series telemetry. |
| **8123** | TCP | **ClickHouse HTTP Interface** | REST API used by Grafana plugins, Web UIs, and HTTP client libraries. |
| **9000** | TCP | **ClickHouse Native TCP** | Binary protocol used by `clickhouse-client` and high-performance drivers. |
| **9004** | TCP | **ClickHouse MySQL Emulation** | Connect to ClickHouse directly using native MySQL clients and BI tools! |
| **9005** | TCP | **ClickHouse PostgreSQL Emulation**| Connect to ClickHouse directly using native PostgreSQL clients! |
| **9009** | TCP | **ClickHouse Inter-Server Sync** | Data replication port between distributed ClickHouse nodes. |
| **9042** | TCP | **Apache Cassandra / ScyllaDB** | Native binary transport for CQL (Cassandra Query Language). |
| **9200** | TCP | **Elasticsearch / OpenSearch** | RESTful HTTP API for indexing, full-text searching, and cluster status. |
| **9300** | TCP | **Elasticsearch Transport** | Java binary protocol for internal node-to-node transport and cluster sync. |

---

### ☸️ Kubernetes & Container Infrastructure Ports

| Port | Protocol | Component / Service | Traffic Source & Destination |
| :---: | :---: | :--- | :--- |
| **443** | TCP | **Kubernetes Service** | Default port exposed by the `kubernetes` ClusterIP in default namespace |
| **2379** | TCP | **etcd Client** | Kube-apiserver connecting to etcd key-value store |
| **2380** | TCP | **etcd Peer** | High-availability inter-node etcd cluster synchronization |
| **6443** | TCP | **Kubernetes API Server** | Primary secure HTTPS control plane API endpoint (`kube-apiserver`) |
| **8443** | TCP | **K8s Webhooks / Ingress** | Admission webhooks (Validating/Mutating) and Ingress controllers |
| **10250** | TCP | **Kubelet API** | Control plane to worker node (exec, logs, pod lifecycle metrics) |
| **10255** | TCP | **Kubelet Read-Only** | Deprecated unauthenticated metrics port (must be disabled for security) |
| **10257** | TCP | **kube-controller-manager** | Secure internal metrics and health check endpoint |
| **10259** | TCP | **kube-scheduler** | Secure internal metrics and health check endpoint |
| **30000–32767** | TCP / UDP | **NodePort Range** | Default Kubernetes NodePort service allocation range |

---

### 🕸️ Service Mesh, Proxies & Ingress Gateways

| Port | Protocol | Tool / Proxy | Functionality & Purpose |
| :---: | :---: | :--- | :--- |
| **8080** | TCP | **HTTP Alternate / NGINX** | Standard non-root web server or upstream container port |
| **8443** | TCP | **HTTPS Alternate / Traefik** | Non-privileged SSL/TLS reverse proxy listener |
| **15000** | TCP | **Envoy Admin** | Localhost administrative control and configuration endpoint |
| **15001** | TCP | **Envoy Outbound** | Istio sidecar outbound traffic redirection listener (via iptables) |
| **15006** | TCP | **Envoy Inbound** | Istio sidecar inbound traffic capture listener |
| **15010** | TCP | **Istiod Plaintext** | Control plane gRPC configuration distribution (xDS) |
| **15012** | TCP | **Istiod mTLS** | Control plane secure mutual TLS configuration distribution |
| **15021** | TCP | **Envoy Health Probe** | Kubernetes liveness and readiness probe port for Istio sidecars |
| **15090** | TCP | **Envoy Prometheus** | Envoy proxy scrape endpoint exposing raw proxy metrics |

---

### ⚙️ CI/CD, DevOps & Secrets Management

| Port | Protocol | Service / Tool | Purpose & Usage |
| :---: | :---: | :--- | :--- |
| **8080** | TCP | **Jenkins Controller** | Web UI and REST API management interface |
| **8081** | TCP | **SonarQube / Nexus** | Code quality analysis dashboard / Maven/Docker repository manager |
| **8082** | TCP | **JFrog Artifactory** | Binary artifact repository management interface |
| **8200** | TCP | **HashiCorp Vault** | Vault HTTPS API for secret leasing and token management |
| **8201** | TCP | **HashiCorp Vault Cluster** | Vault server-to-server Raft cluster consensus and storage replication |
| **8500** | TCP | **HashiCorp Consul HTTP** | Service discovery and KV configuration HTTP API |
| **8501** | TCP | **HashiCorp Consul HTTPS** | Secure TLS service mesh and configuration API |
| **8600** | UDP / TCP | **HashiCorp Consul DNS** | DNS interface for service discovery lookups |
| **50000** | TCP | **Jenkins Agent (JNLP)** | Inbound build agent connection port to controller |

---

### 📊 Observability, Monitoring & Logging Ports

| Port | Protocol | Tool / Exporter | Observability Role |
| :---: | :---: | :--- | :--- |
| **3000** | TCP | **Grafana** | Unified metrics, logs, and traces dashboard Web UI |
| **3100** | TCP | **Grafana Loki** | Log aggregation and indexing HTTP API |
| **4317** | TCP | **OpenTelemetry (gRPC)** | OTLP telemetry receiver via high-speed gRPC |
| **4318** | TCP | **OpenTelemetry (HTTP)** | OTLP telemetry receiver via standard HTTP/JSON |
| **5044** | TCP | **Logstash Beats** | Ingestion port for Filebeat and Metricbeat log streams |
| **5601** | TCP | **Kibana** | Elasticsearch logging and search analytics visualization UI |
| **9090** | TCP | **Prometheus Server** | Prometheus time-series database UI and PromQL HTTP API |
| **9091** | TCP | **Prometheus Pushgateway** | Ephemeral batch job metric aggregator |
| **9093** | TCP | **Alertmanager** | Prometheus alert deduplication, grouping, and notification routing |
| **9100** | TCP | **Node Exporter** | Host Linux OS hardware and kernel metrics exporter |
| **9115** | TCP | **Blackbox Exporter** | Network endpoint probing (HTTP, HTTPS, DNS, TCP, ICMP) |
| **14250** | TCP | **Jaeger Collector (gRPC)** | Span data ingestion via gRPC |
| **14268** | TCP | **Jaeger Collector (HTTP)** | Span data ingestion via HTTP |
| **16686** | TCP | **Jaeger Query UI** | Distributed tracing search and waterfall visualization UI |
| **24224** | TCP / UDP | **Fluentd** | Fluentd forward protocol for container log routing |

---

### 📨 Message Queues & Distributed Event Streaming

| Port | Protocol | Platform | Protocol & Architectural Function |
| :---: | :---: | :--- | :--- |
| **2181** | TCP | **Apache ZooKeeper** | Distributed coordination and metadata storage for Kafka (legacy) |
| **4222** | TCP | **NATS** | Lightweight cloud-native messaging client connection port |
| **5672** | TCP | **RabbitMQ** | AMQP (Advanced Message Queuing Protocol) client port |
| **8222** | TCP | **NATS Management** | NATS HTTP monitoring and metrics inspection interface |
| **9092** | TCP | **Apache Kafka Plaintext** | Default internal broker listener for producer/consumer traffic |
| **9093** | TCP | **Apache Kafka SSL** | Secure TLS encrypted broker listener |
| **9094** | TCP | **Apache Kafka External** | Public/External SASL_SSL listener for client applications |
| **15672** | TCP | **RabbitMQ Management** | Web UI dashboard and HTTP management API |

---

### 🔒 Remote Desktop, Virtualization & VPN Tunnels

| Port | Protocol | Technology | Protocol & Use Case |
| :---: | :---: | :--- | :--- |
| **500** | UDP | **IPsec IKE** | Internet Key Exchange for establishing VPN security associations |
| **1194** | UDP / TCP | **OpenVPN** | OpenVPN SSL/TLS point-to-point and client-to-site tunnels |
| **3389** | TCP / UDP | **RDP** | Remote Desktop Protocol for Windows Server management |
| **4500** | UDP | **IPsec NAT-T** | IPsec NAT-Traversal encapsulation over UDP |
| **5900** | TCP | **VNC** | Virtual Network Computing remote screen sharing |
| **51820** | UDP | **WireGuard** | Next-generation modern high-speed kernel VPN tunnel |

---

## 4. Essential Linux CLI Port Diagnostic Commands

DevOps engineers must be capable of inspecting listening sockets, identifying process IDs, and testing remote reachability:

```bash
# 1. List all active listening TCP and UDP sockets with Process Name and PID
ss -tulpn

# 2. Check which process is occupying a specific port (e.g. 80, 443, 5432)
lsof -i :80
lsof -i :443
lsof -i :5432

# 3. Alternative: identify and optionally kill process holding a port
fuser -v 8080/tcp
fuser -k 8080/tcp    # Emergency terminate process blocking port

# 4. Fast non-blocking remote port connectivity test without telnet (3s timeout)
nc -zvw3 api.example.com 443

# 5. Bash native TCP port test (no extra tools required!)
(echo > /dev/tcp/10.0.1.50/6443) >/dev/null 2>&1 && echo "Port 6443 is OPEN" || echo "Port 6443 is CLOSED"

# 6. Detailed network port scan using Nmap (TCP SYN Stealth Scan)
nmap -sS -p 22,80,443,3306,5432,6443 192.168.1.100
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - UDP Protocol Architecture and Use Cases](./04-UDP-Protocol-Architecture-and-Use-Cases.md) | [Index](../../../README.md) | [06 - TCP Tuning and Optimization in Linux →](./06-TCP-Tuning-and-Optimization-in-Linux.md) |

