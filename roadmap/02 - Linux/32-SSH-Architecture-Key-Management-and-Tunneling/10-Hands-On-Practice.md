# 10 — Hands-On Practice Labs: SSH Architecture

---

## Lab 1: Configuring SSH Local Port Forwarding

```bash
# Forward local port 8888 to remote example.com port 80 via SSH server
ssh -N -L 8888:example.com:80 user@myserver.com &

# Test accessing remote web page locally
curl -I http://localhost:8888
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple Choice Questions](./11-MCQ.md) |
