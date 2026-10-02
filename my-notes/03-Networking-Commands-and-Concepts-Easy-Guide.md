# 🌐 Networking Commands & Concepts — Simple Kids-Mind Easy Guide

> **Style:** Short 1-line definitions, advantages, and exact screen details you get when you type the command. Easy to memorize!

---

## 🧠 1. Core Networking Concepts (Explained in 1 Simple Line)

| Concept | Simple Definition (Kids Mind) | Real-Life Analogy | Main Advantage |
| :--- | :--- | :--- | :--- |
| **IP Address** | A unique number assigned to your computer so other computers can find and talk to it. | Your home mailing address. | Lets packets find your computer on the internet. |
| **MAC Address** | A permanent physical fingerprint burned into your network card by the factory. | Your government fingerprint ID. | Identifies your exact physical device on local Wi-Fi. |
| **Port Number** | A 16-bit number ($0–65535$) identifying which specific app on a computer gets the message. | An apartment room number inside a big building. | Lets one server run Web (80), Mail (25), and DB (3306) together. |
| **Subnet Mask** | Identifies which part of your IP address belongs to your local room vs the outside world. | The neighborhood boundary fence. | Helps computer know if a target is local or needs a router. |
| **Default Gateway** | The IP address of your Wi-Fi router that sends your traffic out to the internet. | The exit door of your house. | Directs all internet-bound packets out of your local network. |
| **DNS** | The phonebook of the internet that translates names (`google.com`) into numbers (`142.250.190.46`). | Phonebook contacts list. | Humans don't have to memorize billions of IP numbers. |
| **DHCP** | The automated robot inside your router that automatically gives your computer an IP address. | Hotel receptionist handing you a room key. | Zero manual configuration needed to join Wi-Fi. |
| **TCP** | A reliable communication protocol that checks every packet arrives in order without errors. | Registered post requiring a delivery signature. | Guaranteed delivery for web pages, files, and databases. |
| **UDP** | A super-fast protocol that fires packets continuously without waiting for confirmations. | Live TV broadcast. | Zero delay for video streaming, gaming, and DNS lookups. |
| **NAT** | Hides 50 home devices behind 1 single public internet IP address. | Apartment mailroom distributing letters to units. | Saves IPv4 addresses and shields devices from direct attack. |
| **Firewall** | A security security guard checking incoming packets and blocking unapproved ports. | Security guard at the building gate. | Blocks hackers and unauthorized traffic. |

---

### 🧱 The OSI 7-Layer Model (Kids Mind)

| Layer | Name | 1-Line Explanation | Real-Life Analogy | Protocols / Tech |
| :---: | :--- | :--- | :--- | :--- |
| **7** | **Application** | The software layer you and your browser interact with directly. | The letter text you wrote. | HTTP, HTTPS, SSH, DNS, FTP |
| **6** | **Presentation** | Translates, formats, encrypts, and compresses data. | Translating English to French or encrypting secret code. | TLS/SSL, JSON, gzip, JPEG |
| **5** | **Session** | Starts, maintains, and cleanly terminates connections between apps. | Making a phone call and keeping the line open until goodbye. | Sockets, RPC, NetBIOS |
| **4** | **Transport** | Splits data into chunks, manages ports, and guarantees delivery. | Registered Postal Delivery ensuring no letters are lost. | TCP (reliable), UDP (fast) |
| **3** | **Network** | Routes packets across different computers worldwide using IP addresses. | The postal sorting center routing mail by street address. | IP (IPv4/IPv6), ICMP, Routers |
| **2** | **Data Link** | Transfers data between two adjacent devices on the same wire/switch. | Passing a paper note directly to the person sitting next to you. | MAC Address, Ethernet, Wi-Fi |
| **1** | **Physical** | The raw physical electrical pulses, radio waves, or light beams. | The copper wire, radio waves, or glass fiber cable itself. | Cables, Hubs, Radio frequencies |

---

### 🤝 TCP 3-Way Handshake & 4-Way Teardown (1-Line Breakdown)
- **Handshake (Connecting):**
  1. `SYN` (Client ──► Server): *"Hello! May I connect?"*
  2. `SYN-ACK` (Server ──► Client): *"Yes, I am listening! Can you hear me?"*
  3. `ACK` (Client ──► Server): *"I hear you! Connection established. Sending data now!"*
- **Teardown (Disconnecting):**
  1. `FIN`: *"I have finished sending data. Goodbye!"*
  2. `ACK`: *"Understood, closing my side."*
  3. `FIN`: *"I am also finished. Goodbye!"*
  4. `ACK`: *"Goodbye! Connection closed."*

---

### 📐 Subnetting & CIDR Cheat Sheet (Kids Mind)
- **What is CIDR?** Slash notation (`/24`) indicating how many bits represent the network prefix.
- **The Golden Rules:**
  - `/32` = **1 IP** (Single exact machine, e.g., `10.0.1.5/32`).
  - `/28` = **16 IPs** (Small microservice cluster).
  - `/24` = **256 IPs** (Standard subnet; $256 - 5 = 251$ usable in AWS/GCP).
  - `/16` = **65,536 IPs** (Standard enterprise VPC).
  - `/8` = **16,777,216 IPs** (Huge telecom network).
- **Private IP Ranges (Never routable on the public internet - RFC 1918):**
  - `10.0.0.0/8` (10.0.0.0 – 10.255.255.255): Large corporate & cloud VPCs.
  - `172.16.0.0/12` (172.16.0.0 – 172.31.255.255): Docker bridge default network.
  - `192.168.0.0/16` (192.168.0.0 – 192.168.255.255): Home Wi-Fi routers.

---

## 💻 2. The Big 6 Network Commands: What Details Show on Screen

### 1️⃣ `ip addr` (Linux) vs `ipconfig` (Windows)
- **One-line Definition:** Shows the IP addresses, network cards, and connection status of your machine.
- **Advantage:** First command to run to see if your computer is connected to the network.

#### What details you get in Linux (`ip addr` or `ip a`):
```text
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536
    inet 127.0.0.1/8 scope host lo            <── Your internal localhost IP
2: eth0: <BROADCAST,MULTICAST,UP> mtu 1500
    link/ether 00:15:5d:02:1a:3f               <── Hardware MAC Address
    inet 192.168.1.15/24 brd 192.168.1.255     <── Your IPv4 Address & /24 Subnet
    inet6 fe80::215:5dff:fe02:1a3f/64          <── Your IPv6 Address
```

#### What details you get in Windows (`ipconfig`):
```text
Ethernet adapter Ethernet:
   Connection-specific DNS Suffix  . : lan
   IPv4 Address. . . . . . . . . . . : 192.168.1.50   <── Your Local Computer IP
   Subnet Mask . . . . . . . . . . . : 255.255.255.0  <── Subnet Mask
   Default Gateway . . . . . . . . . : 192.168.1.1   <── Wi-Fi Router IP
```

#### What details you get in Windows (`ipconfig /all`):
```text
   Host Name . . . . . . . . . . . . : DEV-LAPTOP
   Physical Address. . . . . . . . . : 3C-52-82-4F-21-9A <── Hardware MAC Address
   DHCP Enabled. . . . . . . . . . . : Yes
   IPv4 Address. . . . . . . . . . . : 192.168.1.50(Preferred)
   Default Gateway . . . . . . . . . : 192.168.1.1
   DHCP Server . . . . . . . . . . . : 192.168.1.1       <── Router IP giving out IPs
   DNS Servers . . . . . . . . . . . : 8.8.8.8           <── Domain name resolver
```

---

### 2️⃣ `ip link` (Linux)
- **One-line Definition:** Shows physical network cards, their cable state (UP or DOWN), and MTU packet size limits.
- **Advantage:** Check if a network cable is connected or virtual adapter is active without seeing IP addresses.
- **What details you get when you type it:**
```text
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc mq state UP
    link/ether 52:54:00:12:34:56 brd ff:ff:ff:ff:ff:ff
```
- `state UP`: The network card is alive and transmitting.
- `mtu 1500`: Maximum Transmission Unit (maximum packet size in bytes).
- `link/ether`: Physical MAC address.

---

### 3️⃣ `ip route` (Linux)
- **One-line Definition:** Shows the routing table and Default Gateway where packets are sent to reach the internet.
- **Advantage:** Know immediately which router your server uses to reach the outside world.
- **What details you get when you type it:**
```text
default via 192.168.1.1 dev eth0 proto dhcp metric 100
192.168.1.0/24 dev eth0 proto kernel scope link src 192.168.1.15
```
- `default via 192.168.1.1`: All internet traffic exits through router `192.168.1.1` on card `eth0`.
- `192.168.1.0/24`: Local devices can be reached directly on the local wire without going through the router.

---

### 4️⃣ `hostname -I` (Linux)
- **One-line Definition:** Prints all IP addresses assigned to this machine on a single short line.
- **Advantage:** Cleanest, fastest command to copy your IP address in bash scripts.
- **What details you get when you type it:**
```text
192.168.1.15 172.17.0.1 10.244.0.1
```
- Shows your main LAN IP, Docker bridge IP, and Kubernetes pod CIDR IP side-by-side.

---

### 5️⃣ `ping` (Linux & Windows)
- **One-line Definition:** Sends a test signal to another computer to check if it is awake and measures the response speed.
- **Advantage:** Immediate confirmation of network reachability.
- **What details you get when you type it:**
```text
64 bytes from 8.8.8.8: icmp_seq=1 ttl=118 time=14.3 ms
```
- `time=14.3 ms`: Latency (round-trip time in milliseconds). Under 30ms is great; over 200ms feels laggy.
- `0% packet loss`: Connection is healthy and stable.

---

### 6️⃣ `ss -tulpn` (Linux) vs `netstat -ano` (Windows)
- **One-line Definition:** Lists all listening port numbers and shows which program is using each port.
- **Advantage:** Instantly find out what application is using port 80, 443, 3306, 5432, or 8080.
- **What details you get when you type it:**
```text
Netid  State   Local Address:Port   Peer Address:Port  Process
tcp    LISTEN  0.0.0.0:80           0.0.0.0:*          users:(("nginx",pid=1420,fd=6))
tcp    LISTEN  0.0.0.0:3306         0.0.0.0:*          users:(("mysqld",pid=980,fd=15))
```
- Shows protocol (`tcp`), port (`:80`, `:3306`), and the exact application name (`nginx`, `mysqld`) and PID!

---

### 7️⃣ `traceroute` (Linux) vs `tracert` (Windows)
- **One-line Definition:** Maps every single router hop between your computer and the target server.
- **Advantage:** Find out exactly where network packets get dropped or delayed across the world.
- **What details you get when you type it:**
```text
 1  192.168.1.1 (192.168.1.1)        2.145 ms   <── Your local home Wi-Fi router
 2  10.20.0.1 (10.20.0.1)            8.432 ms   <── Internet Service Provider (ISP) gateway
 3  142.250.190.46 (google.com)      18.231 ms  <── Destination server
```

---

### 8️⃣ `curl -I` (Inspect HTTP Headers & Status Codes)
- **One-line Definition:** Queries a web server and prints only the HTTP response headers without downloading the full page body.
- **Advantage:** Instantly verify if a website is returning `200 OK`, `301 Redirect`, `403 Forbidden`, `404 Not Found`, or `502 Bad Gateway`.
- **What details you get when you type it:**
```text
HTTP/2 200
content-type: text/html; charset=UTF-8
server: gws
date: Fri, 02 Oct 2026 12:00:00 GMT
```

---

### 9️⃣ `dig` (Domain Information Groper) vs `nslookup`
- **One-line Definition:** Queries DNS servers to discover what IP address, mail server, or text record belongs to a domain.
- **Advantage:** Diagnose DNS propagation and verify if domain records are live.
- **What details you get with `dig google.com`:**
```text
;; ANSWER SECTION:
google.com.   300   IN   A   142.250.190.46
;; Query time: 14 msec
;; SERVER: 8.8.8.8#53(8.8.8.8)
```
- In Windows: `nslookup google.com` prints `Name: google.com, Address: 142.250.190.46`.

---

### 🔟 `tcpdump` (Live Network Packet Sniffer)
- **One-line Definition:** Captures and displays real packets flying across your network card in real-time.
- **Advantage:** The ultimate debugging tool for packet drops, TLS handshake failures, and unexpected traffic.
- **Example command:** `sudo tcpdump -i eth0 -n port 80`
- **What details you get:** `12:01:05.123 IP 192.168.1.15.54321 > 93.184.216.34.80: Flags [S], seq 12345, win 65535`

---

### 1️⃣1️⃣ `nc` / `netcat` (Port Connectivity Checker)
- **One-line Definition:** Tests if a specific TCP or UDP port on a remote server is open and accepting traffic.
- **Advantage:** Faster and cleaner than telnet to test database, SSH, or web connectivity.
- **Example command:** `nc -zv 10.0.0.5 5432` (`-z`: zero I/O / scan only, `-v`: verbose).
- **What details you get:** `Connection to 10.0.0.5 5432 port [tcp/postgresql] succeeded!`

---

## 📇 3. DNS Record Types Explained (Kids Mind)

| DNS Record | Simple 1-Line Definition | Real-Life Analogy | Example |
| :--- | :--- | :--- | :--- |
| **`A`** | Maps a domain name to an **IPv4** address ($32$-bit number). | Street address for a store. | `api.myapp.com ──► 54.210.12.8` |
| **`AAAA`** | Maps a domain name to an **IPv6** address ($128$-bit hexadecimal). | Next-generation global GPS coordinate. | `myapp.com ──► 2001:db8::1` |
| **`CNAME`** | An alias name that points one domain name to another domain name. | Nickname pointing to a person's real name. | `www.myapp.com ──► myapp.com` |
| **`MX`** | Directs incoming emails to the company's mail servers. | Post Office box destination for letters. | `myapp.com ──► aspmx.l.google.com` |
| **`TXT`** | Stores arbitrary text notes used for SPF email security and domain ownership verification. | Security stamp proving identity. | `v=spf1 include:_spf.google.com ~all` |
| **`NS`** | Lists the authoritative Nameserver machines managing the domain's DNS records. | The librarian who holds the master directory. | `ns-1.awsdns.com` |
| **`PTR`** | Reverse DNS: Translates an IP address back into its human domain name. | Looking up a phone number to find who owns it. | `54.210.12.8 ──► ec2-54...aws.com` |

---

## 🔌 4. Master Port Numbers at a Glance (Easy Kid-Mind Table)

| Port Number | Protocol | Common Name | Simple 1-Line Explanation |
| :---: | :---: | :--- | :--- |
| **21** | TCP | **FTP** | Used to transfer raw computer files between machines. |
| **22** | TCP | **SSH** | Secure terminal remote control to log into Linux servers. |
| **25** | TCP | **SMTP** | The mail carrier sending emails between mail servers. |
| **53** | UDP/TCP | **DNS** | The phonebook translating names (`google.com`) into numbers. |
| **67, 68** | UDP | **DHCP** | Automated system assigning IP addresses to devices joining Wi-Fi. |
| **80** | TCP | **HTTP** | Standard cleartext web page browsing. |
| **123** | UDP | **NTP** | Synchronizes clock time across all servers worldwide. |
| **179** | TCP | **BGP** | Routing protocol connecting telecom backbones and cloud networks. |
| **443** | TCP/UDP | **HTTPS** | Encrypted secure web pages with SSL/TLS (UDP 443 powers HTTP/3). |
| **1433** | TCP | **MSSQL** | Microsoft SQL Server database listener. |
| **1521** | TCP | **Oracle DB** | Enterprise Oracle database listener. |
| **2181** | TCP | **ZooKeeper** | Coordination system for Kafka clusters. |
| **2379, 2380**| TCP | **etcd** | Kubernetes database storing all cluster state and secrets. |
| **3000** | TCP | **Grafana** | Visual dashboard showing graphs of server metrics. |
| **3100** | TCP | **Loki** | Log aggregation system for storing server logs. |
| **3306** | TCP | **MySQL** | The world's most popular open-source SQL database. |
| **33060** | TCP | **MySQL X** | MySQL Document Store NoSQL and X DevAPI port. |
| **33061** | TCP | **MySQL MGR** | Group Replication communication for MySQL InnoDB clusters. |
| **33062** | TCP | **MySQL Admin** | Dedicated emergency backdoor port for DBAs when 3306 is full! |
| **3389** | TCP/UDP | **RDP** | Windows Remote Desktop screen sharing and control. |
| **4317, 4318**| TCP | **OpenTelemetry** | Ingests distributed traces and metrics (4317 gRPC, 4318 HTTP). |
| **4567** | TCP/UDP | **Galera** | Multi-master replication traffic for Galera MySQL clusters. |
| **5432** | TCP | **PostgreSQL** | Advanced enterprise relational database. |
| **5672, 15672**| TCP | **RabbitMQ** | Message queue (5672 data, 15672 management web UI). |
| **6032, 6033**| TCP | **ProxySQL** | MySQL connection proxy (6032 admin, 6033 traffic). |
| **6379** | TCP | **Redis** | Lightning-fast in-memory cache and session store. |
| **6432** | TCP | **PgBouncer** | Connection pooler that stops PostgreSQL from running out of RAM. |
| **6443** | TCP | **Kube-API** | The brain of Kubernetes (`kube-apiserver`) receiving `kubectl` commands. |
| **8080** | TCP | **HTTP Alt** | Jenkins CI/CD controller web UI or non-root container web servers. |
| **8200, 8201**| TCP | **Vault** | HashiCorp Vault storing enterprise passwords, API keys, and certificates. |
| **8500, 8600**| TCP/UDP | **Consul** | HashiCorp Consul service discovery (8500 HTTP, 8600 DNS). |
| **9042** | TCP | **Cassandra** | Apache Cassandra distributed NoSQL database. |
| **9090** | TCP | **Prometheus** | Metrics server collecting CPU, RAM, and application metrics. |
| **9092, 9093**| TCP | **Kafka** | High-speed event streaming pipeline (9092 plain, 9093 TLS). |
| **9093** | TCP | **Alertmanager** | Sends Slack and PagerDuty alert notifications when servers fail. |
| **9100** | TCP | **Node Exporter** | Scrapes Linux host CPU and memory stats for Prometheus. |
| **9200, 9300**| TCP | **Elasticsearch** | Search engine (9200 REST API, 9300 internal node sync). |
| **10250** | TCP | **Kubelet** | Node-level daemon allowing control plane to run pods and exec commands. |
| **15001, 15006**| TCP | **Istio / Envoy** | Service mesh sidecar capturing outbound (15001) and inbound (15006) traffic. |
| **16379** | TCP | **Redis Cluster**| Inter-node gossip communication between Redis cluster nodes. |
| **27017** | TCP | **MongoDB** | Standard NoSQL document database listener. |
| **30000–32767**| TCP/UDP | **NodePort** | Default Kubernetes port range for exposing services directly on node IPs. |
| **50000** | TCP | **Jenkins Agent** | Inbound connection port for build worker agents. |
| **51820** | UDP | **WireGuard** | Next-generation ultra-fast kernel VPN tunnel. |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Windows & PowerShell Easy Guide](./02-Windows-PowerShell-CMD-Easy-Guide.md) | [Index](../README.md) | [Docker & Containers Short Notes →](./04-Docker-and-Containers-Short-Notes.md) |
