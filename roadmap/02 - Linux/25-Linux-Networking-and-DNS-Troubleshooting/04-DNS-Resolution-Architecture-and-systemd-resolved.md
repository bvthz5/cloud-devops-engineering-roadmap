# 04 — DNS Resolution Architecture and systemd-resolved

Domain Name System (DNS) resolution is the source of many production incidents in cloud and Kubernetes environments. Understanding how a Linux process resolves `api.internal.company.com` to an IP address requires understanding the resolver pipeline.

---

## 1. The Linux Name Resolution Pipeline

When an application calls `getaddrinfo()` or `gethostbyname()` in C/Go/Python:

```text
Application (e.g. curl https://example.com)
                     ↓
             /etc/nsswitch.conf
    (Configures lookup priority: files, dns, resolve)
         /                       \
        v                         v
  /etc/hosts              DNS Resolver Subsystem
(Static host mapping)               ↓
                            /etc/resolv.conf
                                    ↓
              Local Stub Resolver (127.0.0.53 / systemd-resolved)
              Or Directly to Upstream DNS (e.g., 10.0.0.2, 8.8.8.8)
                                    ↓
                     Upstream Recursive Nameservers
```

---

## 2. `/etc/nsswitch.conf` (Name Service Switch)

The `hosts:` line defines the lookup order:
```text
hosts:          files mdns4_minimal [NOTFOUND=return] dns
```
1. `files`: Checks `/etc/hosts` first. If matching IP found, returns immediately.
2. `dns`: Queries nameservers configured in `/etc/resolv.conf`.

---

## 3. `/etc/resolv.conf` Breakdown

```text
nameserver 127.0.0.53
options edns0 trust-ad
search us-east-1.compute.internal production.local
ndots:5
```

- **`nameserver`**: IP address of the DNS server (up to 3 can be listed).
- **`search`**: Search domain list. If you query an unqualified name (e.g. `redis-master`), the resolver appends each domain sequentially: `redis-master.us-east-1.compute.internal`, then `redis-master.production.local`.
- **`ndots:N`**: If a query has fewer than `N` dots, it searches through search domains first before trying the query as an absolute domain. **In Kubernetes, `ndots:5` by default causes significant DNS query amplification!**

---

## 4. `systemd-resolved` Architecture

On modern Ubuntu and systemd-based Linux systems, `/etc/resolv.conf` is a symlink pointing to `/run/systemd/resolve/stub-resolv.conf`.
The local stub resolver listens on `127.0.0.53:53`.

### Inspecting & Managing DNS with `resolvectl`:
```bash
# View active DNS servers per interface and global status
resolvectl status

# Query DNS resolution through systemd-resolved
resolvectl query google.com

# Flush the local DNS cache
sudo resolvectl flush-caches

# Inspect cache statistics
resolvectl statistics
```

---

## 5. DNS Diagnostics with `dig`

`dig` (Domain Information Groper) is the gold standard DNS diagnostic tool.

```bash
# Basic query
dig example.com

# Query specific nameserver directly (bypassing local resolver)
dig @8.8.8.8 example.com

# Query specific record type (A, AAAA, MX, TXT, CNAME, PTR)
dig example.com MX
dig -x 142.250.190.46   # Reverse DNS (PTR) lookup

# Trace DNS resolution hierarchically from root (.) servers to authoritative
dig +trace example.com

# Short output format (useful for bash scripts)
dig +short example.com
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Socket and Port Inspection](./03-Socket-and-Port-Inspection-ss-and-netstat.md) | [README](./README.md) | [05 - Packet Analysis and Diagnostics](./05-Packet-Analysis-and-Diagnostics-tcpdump-traceroute-ping.md) |
