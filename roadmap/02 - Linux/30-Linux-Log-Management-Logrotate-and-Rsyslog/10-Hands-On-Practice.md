# 10 — Hands-On Practice Labs: Linux Log Management

---

## Lab 1: Configuring Custom Logrotate Policy for Microservices

```bash
# 1. Create a dummy log file
sudo mkdir -p /var/log/lab-service
sudo dd if=/dev/urandom of=/var/log/lab-service/app.log bs=1M count=15

# 2. Write rotation policy
sudo tee /etc/logrotate.d/lab-service << 'EOF'
/var/log/lab-service/*.log {
    size 5M
    rotate 3
    compress
    copytruncate
    missingok
}
EOF

# 3. Test in dry-run mode
sudo logrotate -d /etc/logrotate.d/lab-service

# 4. Force rotation and verify compressed archive!
sudo logrotate -f /etc/logrotate.d/lab-service
ls -lh /var/log/lab-service/
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple Choice Questions](./11-MCQ.md) |
