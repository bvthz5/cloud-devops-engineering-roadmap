# 09 - Troubleshooting Tools: Interview Questions & Answers

### Q1: What does `Recv-Q` greater than 0 indicate on a listening TCP socket in `ss -lnt`?
**Answer:** It indicates that the kernel TCP handshake is completed, but the user-space application has not invoked the `accept()` system call to pull the connection off the queue. This means the application process is frozen, deadlocked, or starved of CPU.

### Q2: What is the difference between an intermediate MTR hop showing 80% loss versus the final hop showing 0% loss?
**Answer:** It indicates ICMP rate limiting on the intermediate router's control plane. Since the final destination experiences 0% loss, all application traffic traverses that intermediate hop without any packet drops.

### Q3: How do you capture only TCP SYN packets with `tcpdump`?
**Answer:**
```bash
sudo tcpdump -nn "tcp[tcpflags] & (tcp-syn) != 0 and tcp[tcpflags] & (tcp-ack) == 0"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
