# 05 - Elastic Load Balancing (ELB): ALB (Layer 7) vs. NLB (Layer 4) vs. GWLB (Layer 3)

> **Focus:** High-availability traffic distribution, Application Load Balancer (ALB), Network Load Balancer (NLB), Gateway Load Balancer (GWLB), SSL termination, Target Groups, and Health Checks.

---

## ⚖️ 1. ALB vs. NLB vs. GWLB Comparison

```text
┌───────────────────────────────┐  ┌───────────────────────────────┐  ┌───────────────────────────────┐
│ Application Load Balancer(ALB)│  │   Network Load Balancer (NLB) │  │  Gateway Load Balancer (GWLB) │
│                               │  │                               │  │                               │
│  • OSI Layer 7 (HTTP/HTTPS)   │  │  • OSI Layer 4 (TCP/UDP/TLS)  │  │  • OSI Layer 3 (IP Packets)   │
│  • Path routing (/api, /img)  │  │  • Ultra-low latency (<1ms)   │  │  • Third-party firewalls &    │
│  • Host header routing        │  │  • Static Elastic IP per AZ   │  │    inline intrusion detection │
│  • SSL/TLS Termination offload│  │  • Millions of req/sec scale  │  │  • GENEVE protocol tunneling  │
└───────────────────────────────┘  └───────────────────────────────┘  └───────────────────────────────┘
```

### 📊 Master ELB Decision Matrix:

| Architectural Feature | Application Load Balancer (ALB) | Network Load Balancer (NLB) |
| :--- | :--- | :--- |
| **OSI Layer** | **Layer 7** (Application) | **Layer 4** (Transport) |
| **Protocols** | HTTP, HTTPS, gRPC, WebSocket | TCP, UDP, TLS |
| **Routing Capability** | URL path, Host headers, HTTP query parameters, cookies | IP address & port (5-tuple hash) |
| **Latency** | Single-digit milliseconds | **Sub-millisecond (Ultra-fast)** |
| **Static IP Address** | ❌ No (Exposes DNS CNAME) | ✅ **Yes (Static Elastic IP per AZ)** |
| **Security / WAF** | ✅ Direct AWS WAF integration | ❌ No direct WAF integration |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - AMI Creation Management and Packer Baking](./04-AMI-Creation-Management-and-Packer-Baking.md) | [Index](../../../README.md) | [06 - Auto Scaling Groups ASG Target Tracking and Predictive Scaling →](./06-Auto-Scaling-Groups-ASG-Target-Tracking-and-Predictive-Scaling.md) |
