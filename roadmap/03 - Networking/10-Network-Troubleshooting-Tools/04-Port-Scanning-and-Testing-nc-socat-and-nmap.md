# 04 - Port Scanning and Testing: nc, socat, and nmap

## 1. Netcat (`nc`): The Swiss Army Knife

```bash
# Check if a remote TCP port is open (Zero I/O mode with 2s timeout)
nc -zv -w 2 10.0.1.50 443

# Check UDP port reachability
nc -zuv -w 2 10.0.1.50 53

# Quick echo server for testing firewalls (Listen on port 9000)
nc -l -p 9000
```

---

## 2. `socat`: Multipurpose Relay and Port Forwarder

```bash
# Forward incoming traffic on port 80 to an internal backend on port 8080
socat TCP-LISTEN:80,fork TCP:127.0.0.1:8080

# Expose a local UNIX domain socket (e.g. Docker daemon) over a TCP port
socat TCP-LISTEN:2375,fork UNIX-CONNECT:/var/run/docker.sock
```

---

## 3. `nmap`: Network Exploration and Security Auditing

```bash
# Fast SYN scan of top 100 ports (Stealth scan)
sudo nmap -sS -F 192.168.1.100

# Full service and OS version detection on specific port
sudo nmap -sV -p 22,80,443 192.168.1.100

# Scan all 65,535 TCP ports
sudo nmap -p- 192.168.1.100
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Socket and Connection Inspection ss and netstat](./03-Socket-and-Connection-Inspection-ss-and-netstat.md) | [Index](../../../README.md) | [05 - HTTP and API Diagnostics curl and dig →](./05-HTTP-and-API-Diagnostics-curl-and-dig.md) |
