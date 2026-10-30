# 12 — Quick Revision Cheat Sheet: TCP & UDP

---

## 1. Quick Reference

- **TCP:** Connection-oriented, ordered, reliable, flow-controlled, congestion-controlled.
- **UDP:** Connectionless, minimal 8-byte header, unacknowledged, wire speed.
- **TIME_WAIT:** Normal active close (60s).
- **CLOSE_WAIT:** Application bug; process failed to close socket.

---

## 2. High-Yield Port Numbers Cheat Sheet

| Category | Service / Protocol | Default Port | Protocol |
| :--- | :--- | :---: | :---: |
| **Web & Security** | HTTP | **80** | TCP |
| | HTTPS / HTTP/3 (QUIC) | **443** | TCP / UDP |
| | SSH / SFTP | **22** | TCP |
| | DNS | **53** | UDP / TCP |
| | NTP | **123** | UDP |
| | BGP | **179** | TCP |
| **Databases** | MySQL / MariaDB | **3306** | TCP |
| | PostgreSQL | **5432** | TCP |
| | Redis | **6379** | TCP |
| | MongoDB | **27017** | TCP |
| | Elasticsearch | **9200** | TCP |
| **Kubernetes** | Kube-apiserver | **6443** | TCP |
| | etcd client / peer | **2379 / 2380** | TCP |
| | Kubelet API | **10250** | TCP |
| | NodePort Range | **30000–32767**| TCP / UDP |
| **DevOps / CI/CD** | Jenkins Controller / Agent | **8080 / 50000** | TCP |
| | HashiCorp Vault API | **8200** | TCP |
| | HashiCorp Consul API / DNS | **8500 / 8600** | TCP / UDP |
| **Observability** | Prometheus / Alertmanager | **9090 / 9093** | TCP |
| | Grafana / Loki | **3000 / 3100** | TCP |
| | OpenTelemetry (OTLP) | **4317 (gRPC) / 4318 (HTTP)** | TCP |
| | Node Exporter | **9100** | TCP |
| **Message Queues**| Apache Kafka (Plain / SSL / Ext)| **9092 / 9093 / 9094** | TCP |
| | RabbitMQ (AMQP / Mgmt) | **5672 / 15672** | TCP |
| | NATS | **4222** | TCP |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 11 - MCQ](./11-MCQ.md) | [Index](../../../README.md) | [Next Module (06-SSH-and-Secure-Remote-Access) →](../06-SSH-and-Secure-Remote-Access/01-SSH-Protocol-Architecture-and-Handshake.md) |
