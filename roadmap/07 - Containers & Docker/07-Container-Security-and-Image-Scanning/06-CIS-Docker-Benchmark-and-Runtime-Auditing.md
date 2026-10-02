# 06 - CIS Docker Benchmark and Runtime Auditing

## 1. CIS Docker Benchmark

The Center for Internet Security (CIS) maintains an industry-standard security hardening benchmark for Docker hosts and containers.

```bash
# Run automated CIS Docker Benchmark audit
git clone https://github.com/docker/docker-bench-security.git
cd docker-bench-security
sudo sh docker-bench-security.sh
```

---

## 2. Immutable Containers (`--read-only`)

Prevent attackers or malicious scripts from writing backdoors or downloading malware into the container filesystem by enforcing a **read-only root filesystem**:

```bash
docker run -d \
  --name immutable_web \
  --read-only \
  --tmpfs /tmp:rw,noexec,nosuid \
  -p 8080:80 \
  nginx:alpine
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Static Image Vulnerability Scanning](./05-Static-Image-Vulnerability-Scanning.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
