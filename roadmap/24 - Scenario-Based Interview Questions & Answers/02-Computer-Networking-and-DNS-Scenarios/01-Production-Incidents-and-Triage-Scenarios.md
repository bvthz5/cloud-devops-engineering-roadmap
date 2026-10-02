# Computer Networking & DNS Interview Scenarios: Production Incidents & Triage Scenarios

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 1: Intermittent 5-Second DNS Latency in Kubernetes from ndots:5

### 🚨 The Production Scenario
Microservices in a Kubernetes cluster experience intermittent 5-second connection delays when querying external endpoints (e.g., `api.stripe.com`). Internal service discovery works fine.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
By default, `/etc/resolv.conf` in Kubernetes pods specifies `options ndots:5` and multiple search domains (`default.svc.cluster.local`, `svc.cluster.local`, `cluster.local`). Any domain with fewer than 5 dots triggers sequential search queries through all local domains before querying external DNS. Furthermore, Linux glibc sends A and AAAA queries concurrently over UDP; under packet drop or conntrack race conditions, one query times out, causing a fixed 5-second glibc resolver retransmission delay.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Check pod `/etc/resolv.conf` configuration via `kubectl exec -it <pod> -- cat /etc/resolv.conf`.
- Step 2: Trace live DNS lookups inside the container using `dig +trace api.stripe.com` or `tcpdump -nn -i eth0 port 53`.
- Step 3: Append a trailing dot to fully qualified domain names in application config (e.g. `api.stripe.com.`) to bypass search domains.
- Step 4: Configure pod `dnsConfig` to set `options ndots:2`.
- Step 5: Deploy `NodeLocal DNSCache` to cache DNS queries locally on each node and eliminate UDP conntrack race conditions.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Verify current ndots configuration and search paths in pod
kubectl exec <POD> -- cat /etc/resolv.conf

# Test query latency and response flags from inside the pod
kubectl exec <POD> -- dig +time=2 +tries=1 api.stripe.com

# Capture live DNS traffic to inspect search path iterations and 5s timeout spikes
tcpdump -i any -nn port 53

# Verify CoreDNS CPU and memory utilization
kubectl top pods -n kube-system -l k8s-app=kube-dns

# Restart or verify NodeLocal DNSCache agent across cluster
kubectl rollout restart daemonset node-local-dns -n kube-system

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "The 5-second DNS delay in Kubernetes is a classic symptom of the `ndots:5` configuration combined with Linux kernel UDP conntrack race conditions. Because external domains have fewer than 5 dots, glibc queries local cluster suffixes first. If a packet drops, glibc waits exactly 5 seconds before retrying. I resolve this by appending a trailing dot to external FQDNs, reducing `ndots` to 2 in pod specs, and deploying NodeLocal DNSCache across all nodes to serve queries over TCP/local sockets."

---

## 📌 Scenario 2: Asymmetric Routing Causing Packet Drops across Cloud Interconnect

### 🚨 The Production Scenario
Traffic from on-premises servers to an AWS VPC succeeds, but return traffic fails or connections abruptly stall after initial handshake.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Asymmetric routing occurs when packets take one network path outbound (e.g., Direct Connect) and return via a different path (e.g., Site-to-Site VPN or secondary router). Stateful firewalls or Linux hosts with strict Reverse Path Filtering (`rp_filter=1`) detect return packets arriving on an interface different from the route table entry for the source IP, and silently drop the packets as potential spoofing attempts.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Trace bidirectional routes using `traceroute` from both on-prem and cloud endpoints.
- Step 2: Check Linux kernel reverse path filtering settings via `sysctl net.ipv4.conf.all.rp_filter`.
- Step 3: Inspect firewall session state tables and drop counters (`iptables -vnL` or AWS VPC Flow Logs).
- Step 4: Switch `rp_filter` from strict mode (1) to loose mode (2) on multi-homed routers/gateways.
- Step 5: Enforce symmetrical routing via BGP AS-path prepending or BGP Local Preference.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Check if strict reverse path filtering (mode 1) is enabled
sysctl net.ipv4.conf.all.rp_filter

# Enable loose reverse path filtering (mode 2) to permit valid asymmetric return paths
sudo sysctl -w net.ipv4.conf.all.rp_filter=2; sudo sysctl -w net.ipv4.conf.default.rp_filter=2

# Capture packets on all interfaces to observe ingress vs egress interface divergence
tcpdump -nn -i any host 10.50.0.10 and port 443

# Verify which network interface the kernel selects for outbound traffic to destination
ip route get 10.50.0.10

# Inspect active stateful firewall connection tracker entries
conntrack -L -p tcp --dport 443

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Asymmetric routing becomes a breaking issue when stateful network devices or Linux nodes enforce strict Reverse Path Filtering (`rp_filter=1`). The kernel checks whether the incoming packet's source IP would be routed out through the same interface it arrived on; if not, it drops it. I identify this using VPC Flow Logs and `tcpdump`, remediate by setting `rp_filter=2` (loose mode) on multi-homed network gateways, and align BGP Local Preference and AS-path prepending to ensure symmetrical flow."

---

## 📌 Scenario 3: HTTP 504 Gateway Timeout between Load Balancer and Backend Pods

### 🚨 The Production Scenario
An AWS ALB or Azure App Gateway returns intermittent HTTP 504 Gateway Timeout errors during regular traffic, while backend pods report 200 OK.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
HTTP Keep-Alive idle timeout mismatch. If the backend application or reverse proxy (e.g., Nginx or Node.js) has an idle timeout shorter than the cloud Load Balancer's idle timeout (e.g., backend at 60s, ALB at 60s or 65s), the backend closes the TCP socket with a FIN/RST packet at the exact millisecond the ALB dispatches a new incoming client request on that re-used connection.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Compare Load Balancer idle timeout with backend application keepalive timeout.
- Step 2: Check ALB access logs: filter for `target_status_code` vs `elb_status_code` (ALB returns 504 while target is `-`).
- Step 3: Ensure backend server keepalive timeout is strictly greater than the Load Balancer timeout (e.g., ALB at 60s, backend at 65s or 75s).
- Step 4: Verify Nginx `keepalive_timeout 75s;` and Node.js `server.keepAliveTimeout = 65000`.
- Step 5: Validate fix by sending consecutive requests over an open keep-alive TCP session.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Verify keep-alive behavior and response headers from client
curl -iv --keepalive-time 60 https://api.mycompany.com/health

# Check if 504 errors originate from backend web servers or upstream proxy
grep -E '504' /var/log/nginx/access.log

# Inspect Nginx keepalive timeout parameter
cat /etc/nginx/nginx.conf | grep -i keepalive_timeout

# Query AWS ALB idle timeout configuration value
aws elbv2 describe-load-balancer-attributes --load-balancer-arn <ARN>

# Inspect socket connection states and timers on backend instances
ss -tan '( sport = :80 or sport = :443 )'

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "An HTTP 504 where the load balancer returns the timeout while the backend logs show nothing is almost always an idle timeout race condition. If the backend has a 60-second keep-alive timeout and closes the socket just as the load balancer reuses it, the request hits a closing connection and times out. The golden architectural rule is: backend keepalive timeout must always be configured at least 5-15 seconds higher than the upstream load balancer's timeout."

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Linux & Systems Engineering Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet](../01-Linux-and-Systems-Engineering-Scenarios/04-Rapid-Fire-Interview-Drills-and-Cheat-Sheet.md) | [Index](../../../README.md) | [Computer Networking & DNS Interview Scenarios: Architecture & Scaling Scenarios →](02-Architecture-and-Scaling-Scenarios.md) |

