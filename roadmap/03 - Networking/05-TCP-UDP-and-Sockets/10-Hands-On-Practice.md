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
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
