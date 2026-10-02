# 10 - Hands-On Practice Labs

## Lab 1: Hardened Docker Daemon Configuration

### Objective
Configure the Docker daemon with automatic log rotation, live-restore enabled, and userland-proxy disabled.

### Implementation
```bash
# 1. Write production daemon.json
sudo tee /etc/docker/daemon.json <<EOF
{
  "log-driver": "json-file",
  "log-opts": {
    "max-size": "10m",
    "max-file": "3"
  },
  "live-restore": true,
  "userland-proxy": false
}
EOF

# 2. Reload daemon without dropping containers
sudo systemctl reload docker

# 3. Verify settings active
docker info | grep -E "(Logging Driver|Live Restore)"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
