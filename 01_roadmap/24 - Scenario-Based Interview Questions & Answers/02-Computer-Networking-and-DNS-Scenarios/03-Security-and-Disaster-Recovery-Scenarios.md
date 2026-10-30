# Computer Networking & DNS Interview Scenarios: Security & Disaster Recovery Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 7: SSL/TLS Handshake Latency Spike and OCSP Stapling Failure

### 🚨 The Production Scenario
During a global campaign, HTTPS connection establishment time surges from 30ms to over 1,500ms on an Nginx reverse proxy.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
When clients establish a TLS connection, their browsers contact the Certificate Authority's Online Certificate Status Protocol (OCSP) server to verify revocation. If the CA's OCSP responder is overloaded or rate-limiting, every client TLS handshake blocks waiting for CA response. If Nginx OCSP Stapling is misconfigured or unable to reach upstream DNS, handshakes degrade catastrophically.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Benchmark TLS handshake time using `openssl s_client -connect domain.com:443 -status`.
- Step 2: Check if OCSP response is cached and stapled (`OCSP Response Status: successful`).
- Step 3: Configure `ssl_stapling on;` and `ssl_stapling_verify on;` in Nginx.
- Step 4: Set internal trusted resolver in Nginx (`resolver 8.8.8.8 1.1.1.1 valid=300s;`).
- Step 5: Enable TLS 1.3 to reduce round-trips from 2-RTT to 1-RTT (and 0-RTT for session resumption).

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Verify if server provides a valid stapled OCSP response
openssl s_client -connect myapp.com:443 -status -tlsextdebug < /dev/null | grep -A 10 'OCSP response'

# Measure precise breakdown of TCP vs TLS handshake durations
curl -w 'TCP: %{time_connect}s | TLS: %{time_appconnect}s | Total: %{time_total}s\n' -so /dev/null https://myapp.com

# Manually query CA OCSP responder to test reachability and latency
openssl ocsp -issuer chain.pem -cert server.pem -url <OCSP_URL> -CAfile root.pem

# Verify Nginx configuration syntax after enabling OCSP stapling directives
nginx -t

# Reload web server to activate stapled certificates without dropping connections
systemctl reload nginx

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "High TLS handshake latency is frequently caused by missing OCSP stapling, forcing client browsers to synchronously query third-party Certificate Authority responders over the internet. I verify this via `openssl s_client -status`. To resolve it, I configure Nginx with `ssl_stapling on`, configure a high-availability internal DNS resolver, enable TLS 1.3 for 1-RTT handshakes, and configure TLS session tickets for fast session resumption."

---

## 📌 Scenario 8: WebSocket Connections Terminating Every 60 Seconds through Load Balancer

### 🚨 The Production Scenario
Users of an interactive chat application complain that real-time notifications disconnect and reconnect constantly every minute.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
WebSockets maintain long-lived stateful TCP connections over standard HTTP ports. Reverse proxies and Cloud Load Balancers enforce default idle timeouts (e.g., AWS ALB at 60s, Nginx `proxy_read_timeout 60s`). If no data is transmitted within 60 seconds and application-level ping/pong heartbeats are absent, the proxy unilaterally terminates the connection.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check proxy logs for connection termination reasons (e.g., `client closed connection` vs `upstream timed out`).
- Step 2: Increase Nginx `proxy_read_timeout` and `proxy_send_timeout` to 3600s for `/ws` endpoints.
- Step 3: Ensure `Upgrade` and `Connection 'upgrade'` headers are forwarded correctly.
- Step 4: Implement client/server application-level ping/pong heartbeats every 25 seconds.
- Step 5: Configure cloud load balancer idle timeout accordingly.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Connect via CLI WebSocket client and monitor disconnect timestamp
wscat -c wss://api.myapp.com/ws

# Verify Nginx proxy_read_timeout setting for WebSocket location blocks
grep -i 'timeout' /etc/nginx/conf.d/websocket.conf

# Trace whether client, load balancer, or backend initiates TCP FIN termination
tcpdump -i any 'tcp[tcpflags] & (tcp-fin|tcp-rst) != 0 and port 8080'

# Adjust AWS ALB idle timeout to accommodate long-lived sessions
aws elbv2 modify-load-balancer-attributes --load-balancer-arn <ARN> --attributes Key=idle_timeout.timeout_seconds,Value=300

# Apply updated proxy timeout settings safely
systemctl reload nginx

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Frequent 60-second WebSocket disconnects occur because reverse proxies and cloud ALBs apply standard HTTP idle timeouts (60s default) to stateful streams. I resolve this by configuring Nginx `proxy_read_timeout 3600s;` and `proxy_send_timeout 3600s;` specifically on WebSocket paths, and implementing application-level ping/pong frames every 25 seconds to keep intermediary firewall NAT and load balancer state tables perpetually refreshed."

---

## 📌 Scenario 9: BGP Route Flapping and Packet Loss across Multi-Cloud Interconnects

### 🚨 The Production Scenario
Traffic between an AWS VPC and on-premises datacenter suffers 30% packet loss every 3 minutes. BGP neighbor status oscillates between `Established` and `Active/Idle`.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
BGP route flapping is triggered when keepalive packets (`HoldTime` default 90s, keepalive 30s) are dropped due to MTU mismatches, intermediate interface errors, or aggressive BGP dampening algorithms. If an edge router withdraws and readvertises prefixes repeatedly, upstream carrier routers penalize the route and suppress it for a dampening penalty window.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Review BGP state transitions and log events via router CLI (`show ip bgp summary`, `show log`).
- Step 2: Check interface error counters (`CRC errors`, `drops`) on physical Direct Connect / Interconnect ports.
- Step 3: Tune BGP Keepalive and Hold timers to tolerant values (e.g., Keepalive 10s, HoldTime 30s with BFD for sub-second failure detection).
- Step 4: Enable Bidirectional Forwarding Detection (BFD) to offload link-health tracking from BGP control plane.
- Step 5: Verify route stabilization and ensure dampening penalties expire.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check BGP peer state, prefix counts, uptime, and state changes (PfxRcd)
vtysh -c 'show ip bgp summary'

# Inspect BGP neighbor keepalive timer, hold time, and flap statistics
vtysh -c 'show ip bgp neighbors 169.254.0.1'

# Display all active kernel routing table entries learned via BGP protocol
sudo ip route show proto bgp

# Capture BGP control plane packets on TCP port 179 to inspect Keepalive/Notification frames
tcpdump -i eth1 port 179

# Check physical interface hardware error statistics
ethtool -S eth1 | grep -iE 'crc|err|drop'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "BGP route flapping indicates that BGP keepalive packets are missing their hold-time deadlines, prompting peers to tear down the session and flush routing tables. I inspect `show ip bgp summary` and interface CRC error counters. To resolve flapping, I align BGP keepalive/hold timers, configure Bidirectional Forwarding Detection (BFD) for millisecond-level failure detection without control plane stress, and address any MTU or physical link degradation."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Computer Networking & DNS Interview Scenarios: Architecture & Scaling Scenarios](02-Architecture-and-Scaling-Scenarios.md) | [Index](../../../README.md) | [Computer Networking & DNS Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet →](04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) |

