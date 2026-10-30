# 05 - HTTP and API Diagnostics: curl and dig

## 1. High-Precision API Timing with `curl`

Modern SREs use `curl`'s write-out (`-w`) formatting to measure exact microsecond latencies across the connection life cycle:

```bash
curl -w "
   DNS Lookup:        %{time_namelookup}s
   TCP Connect:       %{time_connect}s
   TLS Handshake:     %{time_appconnect}s
   Pre-transfer:      %{time_pretransfer}s
   Start Transfer:    %{time_starttransfer}s (TTFB)
   -------------------------------------------------
   Total Duration:    %{time_total}s
   HTTP Code:         %{http_code}
" -o /dev/null -s "https://api.example.com/v1/health"
```

### Resolving Local Overrides without `/etc/hosts`
Test an upstream backend server directly without touching public DNS:
```bash
# Syntax: --resolve <hostname>:<port>:<target_ip>
curl -Iv https://example.com --resolve example.com:443:10.0.5.22
```

---

## 2. Advanced DNS Tracing with `dig`

```bash
# Perform a full recursive trace from root nameservers down to authoritative servers
dig +trace example.com

# Query specific nameserver directly
dig @1.1.1.1 example.com A +short

# Reverse DNS lookup (PTR)
dig -x 8.8.8.8 +short
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Port Scanning and Testing nc socat and nmap](./04-Port-Scanning-and-Testing-nc-socat-and-nmap.md) | [Index](../../../README.md) | [06 - Bandwidth and Latency Testing iperf3 and ping →](./06-Bandwidth-and-Latency-Testing-iperf3-and-ping.md) |
