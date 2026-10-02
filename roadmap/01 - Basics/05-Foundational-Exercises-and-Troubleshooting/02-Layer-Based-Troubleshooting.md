# Layer-Based Troubleshooting Framework

## 1. General Engineering Troubleshooting Cycle

```text
       Symptom Reported
             ↓
     Collect Evidence (Logs, Metrics, Status)
             ↓
     Identify Affected Layer
             ↓
     Formulate Hypothesis
             ↓
     Test Hypothesis with Diagnostic Command
             ↓
     Find Root Cause ───> Apply Fix
             ↓
     Verify Resolution ───> Document Lessons
```

---

## 2. The Bottom-Up Troubleshooting Hierarchy

When an application fails, systematically isolate issues from the physical/virtual infrastructure layer upward:

```text
    Layer 8: Application Dependencies (DB, Third-party APIs)
       ↑
    Layer 7: Configuration & Secrets (YAML, ENV, Vault)
       ↑
    Layer 6: Application Code & Process (App crashes, Exception, Heap)
       ↑
    Layer 5: Port Listening & Service Daemon (`netstat`, `systemctl`)
       ↑
    Layer 4: Network Connectivity (`ping`, `curl`, Firewall, DNS)
       ↑
    Layer 3: Kernel & OS Resources (OOM Killer, Memory, CPU throttling)
       ↑
    Layer 2: Storage & Filesystem (Disk Full, Inode exhaustion)
       ↑
    Layer 1: Hardware & Hypervisor (VM state, CPU thermal, PCIe/NIC)
```

---

## 3. Real-World Scenario Triage Example

**Symptom:** Website returns `502 Bad Gateway` or `Connection Refused`.

1. **Layer 1 (Hypervisor/Server):** Is the host VM online and responding? (`ping 10.0.0.5`)
2. **Layer 2 (Storage):** Is disk space or inodes full? (`df -h`, `df -i`)
3. **Layer 3 (OS/Kernel):** Did OOM killer terminate the app process? (`dmesg -T | grep -i oom`)
4. **Layer 4 (Network):** Is the firewall blocking port 80/443? (`ufw status`, `iptables -L`)
5. **Layer 5 (Service Daemon):** Is the web server process listening? (`sudo netstat -tulpn | grep 80`)
6. **Layer 6 (App Logs):** What do application logs report? (`tail -f /var/log/nginx/error.log`)
7. **Layer 7 (Configuration):** Was a YAML or config file syntax broken during last deployment? (`nginx -t`)
8. **Layer 8 (Dependencies):** Is the backend database reachable? (`nc -zv db.internal 5432`)
