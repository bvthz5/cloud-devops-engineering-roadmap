# 03 — Socket and Port Inspection (ss and netstat)

Investigating open network ports, established connections, and socket buffers is a primary operational task when debugging microservices, database connection pool exhaustion, or service crashes. The `ss` (Socket Statistics) utility is the modern, high-speed replacement for legacy `netstat`.

---

## 1. Why `ss` Outperforms `netstat`

- `netstat` reads and parses `/proc/net/tcp` and `/proc/net/udp` line by line as plaintext files. On high-throughput servers with 100,000+ concurrent connections, running `netstat` consumes significant CPU and can lock the procfs table.
- `ss` uses the Linux kernel `sock_diag` Netlink subsystem. It queries the kernel socket tables directly in binary format, returning results 10x to 50x faster with minimal resource usage.

---

## 2. Essential `ss` Options & Flags

| Flag | Meaning | DevOps Purpose |
| :--- | :--- | :--- |
| `-t` | TCP sockets | Filter exclusively for TCP protocol |
| `-u` | UDP sockets | Filter exclusively for UDP protocol |
| `-l` | Listening sockets | Display ports that are waiting for incoming connections |
| `-a` | All sockets | Display both listening and established/closing connections |
| `-p` | Processes | Show the process name and PID holding the socket (requires `sudo`) |
| `-n` | Numeric | Do not resolve service names or hostnames (avoids DNS delays) |
| `-e` | Extended info | Show socket UID, inode, and security context |
| `-s` | Summary | Display summary statistics of socket counts without listing |

---

## 3. Production Command Cheatsheet

### 3.1 What services are listening on which ports?
```bash
sudo ss -tulpn
```
**Example output breakdown:**
```text
Netid  State   Recv-Q  Send-Q   Local Address:Port   Peer Address:Port  Process
tcp    LISTEN  0       128      0.0.0.0:22           0.0.0.0:*          users:(("sshd",pid=842,fd=3))
tcp    LISTEN  0       511      127.0.0.1:6379       0.0.0.0:*          users:(("redis-server",pid=1204,fd=6))
tcp    LISTEN  0       4096     *:80                 *:*                users:(("nginx",pid=2100,fd=7))
```

### 3.2 Understanding `Recv-Q` and `Send-Q`
The meaning of `Recv-Q` and `Send-Q` depends on the socket state:
- **In `LISTEN` State:**
  - `Send-Q`: The maximum backlog limit configured in the application (e.g. `listen(fd, backlog)`).
  - `Recv-Q`: The number of connections currently completed in the TCP 3-way handshake waiting in the kernel queue for the application to `accept()`. **If `Recv-Q > 0`, the application is slow or overloaded and failing to accept connections fast enough!**
- **In `ESTABLISHED` State:**
  - `Recv-Q`: Number of bytes received from the network but not yet read by the user application.
  - `Send-Q`: Number of bytes queued by the application in kernel memory waiting to be acknowledged by the remote peer.

---

## 4. Filtering Sockets by State & Address

`ss` provides a powerful internal filtering language:

```bash
# Find all established connections to a remote PostgreSQL database on port 5432
ss -tn state established '( dport = :5432 or sport = :5432 )'

# Find all sockets in TIME_WAIT state (indicates high connection churn / missing keepalives)
ss -tn state time-wait

# Find connections from a specific remote IP address
ss -tn dst 10.0.1.25

# Show socket summary overview
ss -s
```

---

## 5. TCP Connection Lifecycle States

```text
[CLOSED] ---> [LISTEN] (Server)
   |              |
   | SYN          | SYN-ACK
   v              v
[SYN_SENT] -> [SYN_RECV]
       \       /
        v     v
     [ESTABLISHED] (Active Data Transfer)
        /       \
       v         v
  (Active Close)  (Passive Close)
   [FIN_WAIT_1]    [CLOSE_WAIT] (App needs to close socket!)
        |               |
   [FIN_WAIT_2]    [LAST_ACK]
        |               |
   [TIME_WAIT]       [CLOSED]
  (60s 2*MSL)
        |
     [CLOSED]
```

> [!WARNING]
> **CLOSE_WAIT Alert:** If you see hundreds of sockets stuck in `CLOSE_WAIT`, it is an application bug. The remote client disconnected, the kernel handled the FIN, but the local software has not called `close()` on the file descriptor.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Modern Network Configuration](./02-Modern-Network-Configuration-iproute2.md) | [README](./README.md) | [04 - DNS Resolution Architecture](./04-DNS-Resolution-Architecture-and-systemd-resolved.md) |
