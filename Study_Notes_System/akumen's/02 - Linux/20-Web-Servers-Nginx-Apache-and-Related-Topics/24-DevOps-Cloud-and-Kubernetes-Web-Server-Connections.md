# 24. DevOps, Cloud & Kubernetes Web Server Connections

## Cloud & Kubernetes Storage/Ingress Abstractions

In modern DevOps and Cloud-Native environments, web servers transition from standalone VMs into containerized ingress proxies and sidecars:

```text
[ Internet / Clients ]
          │
          ▼
[ Cloud Provider Load Balancer (AWS ALB / GCP HTTPS LB) ]
          │
          ▼
[ Kubernetes Nginx Ingress Controller Pods ]
          │ (Parses Ingress Objects & Dynamically Reloads Nginx Config)
          ▼
[ ClusterIP Services ] ──► [ Application Pods (Node / Python / Go) ]
```

## Key Connections

1. **Ingress Controllers:**
   - **Nginx Ingress Controller:** The standard Kubernetes Ingress controller, embedding Nginx inside a controller pod to dynamically parse K8s `Ingress` resources into Nginx server blocks.
2. **Sidecar Proxies & Service Meshes:**
   - **Envoy Proxy:** High-performance C++ proxy used in Service Meshes (Istio, Linkerd) for mTLS, circuit breaking, and telemetry.
3. **Cloud Infrastructure as Code (IaC):**
   - Terraform provisions Cloud Load Balancers and ACM SSL certificates; Ansible configures on-premise Nginx/Apache cluster nodes.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [23 - Scenario Based Troubleshooting Challenges](./23-Scenario-Based-Troubleshooting-Challenges.md) | [README](./README.md) | [25 - Web Server Command and Config Cheat Sheet](./25-Web-Server-Command-and-Config-Cheat-Sheet.md) |
