# 12 - Quick-Revision & Enterprise Cheat Sheet

```bash
# Network Management
docker network create --driver bridge my_net
docker network connect my_net running_container
docker network inspect my_net

# Bind securely to localhost only
docker run -p 127.0.0.1:8080:80 nginx:alpine

# High-Performance Host Networking
docker run --network host my_streaming_app

# Embedded DNS: 127.0.0.11 (Active on user-defined bridges)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (06-Docker-Compose-Multi-Container-Apps) →](../06-Docker-Compose-Multi-Container-Apps/01-Docker-Compose-v2-Specification-and-Architecture.md) |
