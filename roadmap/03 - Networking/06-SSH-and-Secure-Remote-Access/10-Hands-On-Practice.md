# 10 — Hands-On Practice Labs: SSH

---

## Lab 1: Configuring Local Port Forwarding

```bash
# Forward local port 8080 to example.com:80 via remote SSH server
ssh -N -L 8080:example.com:80 user@myserver &
curl -I http://localhost:8080
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple Choice Questions](./11-MCQ.md) |
