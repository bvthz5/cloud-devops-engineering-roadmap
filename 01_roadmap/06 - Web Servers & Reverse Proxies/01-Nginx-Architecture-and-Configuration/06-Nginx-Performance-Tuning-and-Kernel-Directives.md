# 06 - Nginx Performance Tuning and Kernel Directives

## 1. Zero-Copy Data Transfer: `sendfile` and `tcp_nopush`

In standard file serving without zero-copy:
1. Kernel reads file from disk into kernel buffer cache.
2. Data is copied into user-space Nginx application memory buffer.
3. Nginx writes data back to kernel socket buffer.
4. Socket buffer is sent to Network Interface Card (NIC).
*(Total: 4 context switches and 3 memory copies!)*

With `sendfile on`:
Linux kernel transfers bytes directly from the page cache to the network socket, bypassing user-space entirely (**Zero-Copy**).

```text
Standard Read/Write:
[ Disk ] ──► [ Page Cache ] ──► [ User-space Nginx RAM ] ──► [ Socket Buffer ] ──► [ NIC ]

Zero-Copy sendfile:
[ Disk ] ──► [ Page Cache ] ───────────────────────────────► [ Socket Buffer ] ──► [ NIC ]
```

```nginx
# High-Performance Kernel Directives
sendfile on;

# Sends HTTP response headers and beginning of file in a single packet (reduces packet fragmentation)
tcp_nopush on;

# Disables Nagle's algorithm; sends small packets immediately without delay (essential for low-latency APIs)
tcp_nodelay on;
```

---

## 2. Connection Buffers and Timeout Tuning

```nginx
http {
    # Client Request Body & Header Limits
    client_body_buffer_size 128k;
    client_max_body_size 50m;
    client_header_buffer_size 4k;
    large_client_header_buffers 4 16k;

    # Timeouts to prevent Slowloris attacks
    client_body_timeout 12;
    client_header_timeout 12;
    keepalive_timeout 65;
    send_timeout 10;

    # File Descriptor Cache (Drastically reduces stat() system calls)
    open_file_cache max=10000 inactive=30s;
    open_file_cache_valid 60s;
    open_file_cache_min_uses 2;
    open_file_cache_errors on;
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Virtual Hosting and Server Blocks](./05-Virtual-Hosting-and-Server-Blocks.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
