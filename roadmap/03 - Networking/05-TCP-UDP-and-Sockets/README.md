# Module 05: TCP, UDP, and Sockets

Transport Layer protocols govern how host-to-host data streams are delivered. Master the inner workings of the TCP 3-way handshake, connection teardowns, socket states (`TIME_WAIT`, `CLOSE_WAIT`), kernel buffer queues, UDP performance, and Linux socket tuning.

---

## 🎯 Learning Objectives
By the end of this module, you will be able to:
- Dissect the **TCP 3-Way Handshake** and **4-Way Teardown**.
- Understand TCP reliability: **Sliding Windows, Flow Control, and Congestion Control (BBR, Cubic)**.
- Diagnose socket states and identify **`CLOSE_WAIT` application leaks** vs **`TIME_WAIT` port exhaustion**.
- Compare **TCP vs UDP** performance trade-offs for cloud services.
- Understand the **Linux BSD Socket API** (`bind`, `listen`, `accept`, `epoll`).
- Tune high-concurrency TCP kernel parameters (`somaxconn`, `tcp_tw_reuse`).

---

## 📑 Module Index

| # | Topic | Description | Status |
| :-: | :--- | :--- | :-: |
| 01 | [TCP Architecture & Reliability Guarantees](./01-TCP-Architecture-and-Reliability-Guarantees.md) | Sequence numbers, ACKs, flow control, sliding window, and BBR | ✅ Complete |
| 02 | [The TCP 3-Way Handshake & Teardown](./02-The-TCP-3-Way-Handshake-and-Teardown.md) | SYN, SYN-ACK, ACK, ISN calculation, FIN-ACK teardown sequence | ✅ Complete |
| 03 | [TCP Connection States & Socket Queues](./03-TCP-Connection-States-and-Socket-Queues.md) | State machine transitions, TIME_WAIT, CLOSE_WAIT, Recv-Q vs Send-Q | ✅ Complete |
| 04 | [UDP Protocol Architecture & Use Cases](./04-UDP-Protocol-Architecture-and-Use-Cases.md) | Connectionless transport, 8-byte header, DNS, VoIP, and QUIC | ✅ Complete |
| 05 | [Network Sockets & BSD Socket API](./05-Network-Sockets-and-the-BSD-Socket-API.md) | File descriptor abstraction, `socket()`, `listen()`, and `epoll` | ✅ Complete |
| 06 | [TCP Kernel Tuning & Optimization](./06-TCP-Tuning-and-Optimization-in-Linux.md) | `net.ipv4.tcp_tw_reuse`, `somaxconn`, socket buffers, and window scale | ✅ Complete |
| 07 | [Real-World Production Scenarios](./07-Real-World-Scenarios.md) | Ephemeral port exhaustion; SYN flood attack mitigation; socket leaks | ✅ Complete |
| 08 | [Troubleshooting Guide & Runbook](./08-Troubleshooting.md) | Diagnostic socket filtering with `ss -tulpn`, `ss -s`, and buffer monitoring | ✅ Complete |
| 09 | [Interview Q&A](./09-Interview-QA.md) | 10 technical TCP/UDP interview questions for DevOps, SRE, and Systems | ✅ Complete |
| 10 | [Hands-On Practice Labs](./10-Hands-On-Practice.md) | Capture TCP 3-way handshake with tcpdump; test UDP with netcat | ✅ Complete |
| 11 | [Multiple Choice Questions (MCQ)](./11-MCQ.md) | Self-assessment test with detailed answers and technical explanations | ✅ Complete |
| 12 | [Quick Revision Cheat Sheet](./12-Quick-Revision.md) | State diagram summary, TCP header fields, and ss command cheatsheet | ✅ Complete |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [Module 04: HTTP & HTTPS](../04-HTTP-HTTPS-and-Web-Protocols/README.md) | [Networking Master Index](../README.md) | [01 - TCP Architecture](./01-TCP-Architecture-and-Reliability-Guarantees.md) |
