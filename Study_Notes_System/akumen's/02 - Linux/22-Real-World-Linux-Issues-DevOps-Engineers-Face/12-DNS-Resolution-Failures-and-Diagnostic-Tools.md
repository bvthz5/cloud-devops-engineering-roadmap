# 12 — DNS Resolution Failures and Diagnostic Tools

## 1. Scenario
An application fails to connect to database hostname `db.internal.example.com` with error: `Could not resolve host: db.internal.example.com`.

```text
Scenario
   ↓
Symptoms: "Could not resolve host", "Name or service not known", curl/ping fails by hostname but works by IP
   ↓
What could cause it? Incorrect nameserver in /etc/resolv.conf, systemd-resolved failure, nsswitch.conf misconfiguration
   ↓
Diagnostic commands: dig +trace db.internal.example.com, getent hosts db.internal.example.com, nslookup
```

## 2. Diagnostic Investigation Hierarchy

```bash
# 1. Test system NSS resolver stack (simulates libc getaddrinfo)
getent hosts db.internal.example.com

# 2. Query specified DNS server directly using dig
dig db.internal.example.com @8.8.8.8

# 3. Trace full root-to-authoritative DNS lookup chain
dig +trace db.internal.example.com
```

## 3. Resolving Configuration Files

### `/etc/resolv.conf`
```text
nameserver 1.1.1.1
nameserver 8.8.8.8
```

### Resetting `systemd-resolved` Cache
```bash
sudo systemd-resolve --flush-caches
sudo systemd-resolve --statistics
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Firewall & Security Groups](./11-Firewall-Security-Groups-and-Network-Blocking.md) | [README](./README.md) | [13 - Deployment Failures & Rollback](./13-Production-Deployment-Failures-and-Rollback.md) |
