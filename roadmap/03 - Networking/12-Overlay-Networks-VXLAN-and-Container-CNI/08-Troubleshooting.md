# 08 - Overlay Networks & CNI: Troubleshooting Guide

## 1. Inspecting Container Network Namespaces

```bash
# Locate container process ID (PID)
PID=$(crictl inspect --output go-template --template '{{.info.pid}}' <container-id>)

# Enter the container's network namespace directly from host
nsenter -t $PID -n ip addr show
nsenter -t $PID -n ip route
nsenter -t $PID -n ss -tulpn
```

---

## 2. Cilium eBPF Diagnostics with Hubble

```bash
# Check Cilium agent health and eBPF maps
cilium status

# Stream live network traffic flows for a specific pod
cilium monitor --related-to <endpoint-id>

# Observe dropped packets in real-time
hubble observe --verdict DROPPED
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) | [README](./README.md) | [09 - Interview Q&A](./09-Interview-QA.md) |
