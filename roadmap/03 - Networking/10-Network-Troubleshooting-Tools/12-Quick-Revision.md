# 12 - Troubleshooting Tools: Quick Revision Cheat Sheet

## Diagnostic Command Matrix

| Problem | Diagnostic Command |
|---|---|
| Trace packet path & loss | `mtr -n --report 8.8.8.8` |
| View open listening sockets | `ss -tulpn` |
| Trace DNS resolution steps | `dig +trace example.com` |
| Inspect API latency breakdown | `curl -w "@curl-format.txt" -o /dev/null -s https://api.com` |
| Check if remote port is open | `nc -zv -w 2 10.0.1.5 443` |
| Capture TCP RST packets | `sudo tcpdump -nn "tcp[tcpflags] & tcp-rst != 0"` |
| Measure TCP bandwidth | `iperf3 -c 10.0.1.50 -P 4` |
| Find path MTU size | `ping -M do -s 1472 8.8.8.8` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - Self-Assessment MCQ](./11-MCQ.md) | [README](./README.md) | [11 - BGP & Interconnects](../11-BGP-Routing-and-Cloud-Interconnects/README.md) |
