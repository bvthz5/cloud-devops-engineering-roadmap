# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Dissecting HAProxy Timing Logs

Standard HAProxy HTTP log entry:
```text
Oct  2 09:15:32 lb01 haproxy[1234]: 203.0.113.195:54321 [02/Oct/2026:09:15:32.100] http_in api_servers/api01 10/0/1/15/26 200 452 - - ---- 12/12/3/1/0 0/0 "GET /api/v1/users HTTP/1.1"
```

### The 5 Timers Decoded (`10/0/1/15/26`):
- **`10` (`%TR`)**: Time taken to receive the full HTTP request from client (high = slow mobile network).
- **`0` (`%Tw`)**: Time spent waiting in the queue for a server connection slot (high = backend servers are saturated!).
- **`1` (`%Tc`)**: Time taken to establish TCP connection to backend server (high = network latency or SYN drops).
- **`15` (`%Tr`)**: Time taken by backend server to return first response byte (high = slow database/application code).
- **`26` (`%Tt`)**: Total request duration from start to final byte sent to client.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Questions](./09-Interview-QA.md) |
