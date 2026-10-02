# 10 — Hands-On Practice Labs: TCP & UDP

---

## Lab 1: Capturing the TCP 3-Way Handshake

```bash
# In terminal 1: Start tcpdump listening for SYN packets
sudo tcpdump -i any -nn 'tcp[tcpflags] & (tcp-syn) != 0' -c 3

# In terminal 2: Connect to a web server
curl -s http://example.com > /dev/null
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple Choice Questions](./11-MCQ.md) |
