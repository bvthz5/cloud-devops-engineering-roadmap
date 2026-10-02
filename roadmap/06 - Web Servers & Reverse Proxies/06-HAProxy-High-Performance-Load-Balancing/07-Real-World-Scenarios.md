# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Reload Packet Drop Disaster (Master-Worker Mode)

### Context & Incident
A FinTech company deployed an auto-scaling group that reloaded HAProxy configuration whenever a new microservice pod launched. During peak hours, external clients reported intermittent connection timeouts and dropped HTTPS handshakes.

### Root Cause
HAProxy was configured using legacy reload scripts:
`systemctl reload haproxy` which invoked `kill -USR2` without socket transfer.
During high socket churn, the incoming SYN queue on the old listening socket was closed before the new process bound to the socket, causing the Linux kernel to drop incoming SYN packets.

### Architectural Solution: Master-Worker Socket Passing (`-W -S`)
Run HAProxy in modern **Master-Worker mode** with seamless socket transfer:
```bash
haproxy -W -db -S /run/haproxy/master.sock -f /etc/haproxy/haproxy.cfg
```
In Master-Worker mode, the master process holds the open listening sockets continuously and passes file descriptors seamlessly to new child worker processes across reloads with zero dropped packets.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Dynamic Reconfiguration](./06-Dynamic-Reconfiguration-Runtime-API-and-Data-Plane-API.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
