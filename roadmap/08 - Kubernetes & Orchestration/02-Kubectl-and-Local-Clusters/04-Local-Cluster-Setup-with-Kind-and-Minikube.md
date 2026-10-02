# 04 - Local Cluster Setup with Kind and Minikube

## 1. Kind (Kubernetes in Docker)

**Kind** runs each Kubernetes node as a standalone Docker container. It is the gold standard for CI/CD integration testing and multi-node local simulation.

### Multi-Node Topology Manifest (`kind-cluster.yaml`)
```yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
  extraPortMappings:
  - containerPort: 80
    hostPort: 80
    protocol: TCP
  - containerPort: 443
    hostPort: 443
    protocol: TCP
- role: worker
  labels:
    tier: frontend
- role: worker
  labels:
    tier: backend
```

```bash
# Create cluster from manifest
kind create cluster --name enterprise-lab --config kind-cluster.yaml

# Verify nodes
kubectl get nodes -o wide
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Kubectl Plugins and Krew Ecosystem](./03-Kubectl-Plugins-and-Krew-Ecosystem.md) | [Index](../../../README.md) | [05 - Lightweight Edge Kubernetes with K3s and MicroK8s →](./05-Lightweight-Edge-Kubernetes-with-K3s-and-MicroK8s.md) |
