# 06 - Dynamic Reconfiguration: Runtime API and Data Plane API

## 1. The Runtime CLI Socket

HAProxy provides a high-speed Unix domain management socket. Through this socket, automation scripts and CI/CD pipelines can modify live routing state **without reloading HAProxy or dropping TCP connections**:

```bash
# Install socat utility
sudo apt-get install -y socat

# Query overall HAProxy health and metrics
echo "show info" | sudo socat stdio /run/haproxy/admin.sock

# View real-time status of all backend servers
echo "show servers state" | sudo socat stdio /run/haproxy/admin.sock

# Change backend server weight dynamically (traffic shifting)
echo "set weight api_servers/api01 50%" | sudo socat stdio /run/haproxy/admin.sock

# Inspect live stick-table rate limiting table
echo "show table fe_web" | sudo socat stdio /run/haproxy/admin.sock
```

---

## 2. HAProxy Data Plane API

For declarative, modern microservices automation, HAProxy provides the official **Data Plane API**—a lightweight HTTP/REST daemon running alongside HAProxy:

```bash
# Add a new server dynamically via REST API
curl -X POST -u admin:adminpwd \
  http://127.0.0.1:5555/v2/services/haproxy/configuration/servers?backend=api_servers \
  -H "Content-Type: application/json" \
  -d '{"name": "api03", "address": "10.0.1.12", "port": 8080, "check": "enabled"}'
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Active Health Checks and Graceful Failover](./05-Active-Health-Checks-and-Graceful-Failover.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
