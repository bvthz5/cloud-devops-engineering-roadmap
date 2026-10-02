# 03 - Socket and Connection Inspection: ss and netstat

## 1. Why `ss` Replaces `netstat`

`netstat` reads sockets by parsing `/proc/net/tcp` sequentially. On systems with 50,000+ connections, `netstat` hangs and consumes massive CPU.
`ss` uses the Linux kernel's **Netlink subsystem** to dump socket structures directly from memory in a fraction of a millisecond.

---

## 2. Essential `ss` Commands

```bash
# List all TCP listening sockets with process names and port numbers
ss -tulpn

# Count connections by TCP state
ss -s

# Filter sockets in specific state
ss -t state established
ss -t state time-wait
ss -t state close-wait
```

---

## 3. Interpreting `Send-Q` and `Recv-Q`

Understanding socket queues is critical for troubleshooting saturated applications:

| Queue | For Listening Sockets (`LISTEN`) | For Established Sockets (`ESTABLISHED`) |
|---|---|---|
| **`Recv-Q`** | Sockets waiting in the kernel accept queue. If $>0$, **the application process is stuck or overloaded and not calling `accept()`!** | Bytes received into the OS kernel TCP buffer waiting to be read by the application. |
| **`Send-Q`** | Maximum capacity of the accept queue (`backlog` setting). | Bytes queued in the local network buffer waiting to be sent/ACKed over the network. |

```bash
ss -lnt
State   Recv-Q   Send-Q   Local Address:Port   Peer Address:Port
LISTEN  128      128      0.0.0.0:8080         0.0.0.0:*
# If Recv-Q == Send-Q, the app is dropping new incoming TCP connections!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Path Diagnostics](./02-Path-Diagnostics-traceroute-mtr-and-iproute2.md) | [README](./README.md) | [04 - Port Testing](./04-Port-Scanning-and-Testing-nc-socat-and-nmap.md) |
