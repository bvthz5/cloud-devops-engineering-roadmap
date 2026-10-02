# 03 — TCP Connection States and Socket Queues

Understanding TCP states and socket queues is essential for diagnosing connection bottlenecks.

---

## 1. Deep Dive: `TIME_WAIT` vs `CLOSE_WAIT`

- **`TIME_WAIT` (Normal - Active Close):**
  - The local endpoint sent the first `FIN`.
  - Remains in `TIME_WAIT` for 60 seconds (2 * MSL) to ensure delayed packets clear the network.
  - **Issue:** Thousands of `TIME_WAIT` sockets consume ephemeral ports. Fix with `net.ipv4.tcp_tw_reuse = 1`.
- **`CLOSE_WAIT` (Application Bug - Passive Close):**
  - The remote client disconnected, but the **local application process forgot to call `close()`** on the socket descriptor.
  - **Issue:** Sockets leak indefinitely until the process crashes. Fix: Patch application code.

---

## 2. Listening Socket Queues (`Recv-Q` vs `Send-Q`)

Run `ss -tulpn`:
- **`Send-Q`:** The listen backlog limit (set by `listen(fd, backlog)`).
- **`Recv-Q`:** The count of completed 3-way handshakes waiting for the application to call `accept()`.
- **Alert:** If `Recv-Q > 0`, the application is overloaded and failing to accept connections fast enough!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - TCP Handshake & Teardown](./02-The-TCP-3-Way-Handshake-and-Teardown.md) | [README](./README.md) | [04 - UDP Architecture](./04-UDP-Protocol-Architecture-and-Use-Cases.md) |
