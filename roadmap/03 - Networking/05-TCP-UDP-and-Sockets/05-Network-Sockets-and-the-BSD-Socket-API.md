# 05 — Network Sockets and the BSD Socket API

In Linux, "Everything is a file," including network connections. A **Socket** is an endpoint for communication identified by a file descriptor.

---

## 1. The Server Socket Lifecycle

```text
socket()   ──► Creates an unbound socket descriptor
   │
   ▼
bind()     ──► Binds socket to local IP and Port (e.g. 0.0.0.0:8080)
   │
   ▼
listen()   ──► Marks socket as passive (ready to accept incoming connections)
   │
   ▼
accept()   ──► Blocks until a client completes 3-way handshake; returns NEW socket FD!
   │
   ▼
read() / write() ──► Transfers application payload
   │
   ▼
close()    ──► Closes file descriptor and initiates TCP teardown
```

---

## 2. High-Concurrency Event Notification: `epoll`

In legacy servers, checking 10,000 sockets required scanning an array using `select()` ($O(N)$).
Linux **`epoll`** operates in **$O(1)$** time by having the kernel wake up worker threads only when a socket's state changes. Powers NGINX, Node.js, Envoy, and Redis.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - UDP Protocol Architecture and Use Cases](./04-UDP-Protocol-Architecture-and-Use-Cases.md) | [Index](../../../README.md) | [06 - TCP Tuning and Optimization in Linux →](./06-TCP-Tuning-and-Optimization-in-Linux.md) |
