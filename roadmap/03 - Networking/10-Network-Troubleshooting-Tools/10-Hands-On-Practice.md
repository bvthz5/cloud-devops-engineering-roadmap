# 10 - Hands-On Practice: Diagnosing Connection Drops with tcpdump

## Lab Scenario
Simulate a failed connection and capture the exact packet exchange to differentiate between a **Firewall Drop (Silent Timeout)** and a **Port Closed (TCP RST)**.

---

## Lab Steps

### Step 1: Start tcpdump Capture in Terminal 1
```bash
sudo tcpdump -nn -i any "tcp port 8888"
```

### Step 2: Attempt Connection to Closed Port in Terminal 2
```bash
nc -zv 127.0.0.1 8888
```
Observe the output in Terminal 1:
```
IP 127.0.0.1.54321 > 127.0.0.1.8888: Flags [S], seq ...
IP 127.0.0.1.8888 > 127.0.0.1.54321: Flags [R.], seq 0, ack 1 ...
```
Notice the immediate `[R.]` (TCP Reset) flag sent by the kernel because no process is listening on that port!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Self-Assessment MCQ](./11-MCQ.md) |
