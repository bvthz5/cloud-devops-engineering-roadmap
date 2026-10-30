# 10 - Hands-On Practice Labs

## Lab: Bootstrapping a Multi-Node Kind Cluster with Node Labels

### Objective
Create a local multi-node cluster with 1 control plane and 2 worker nodes, applying custom labels to simulate production node pools.

### Steps:
```bash
# 1. Create kind configuration file
cat << 'EOF' > kind-multinode.yaml
kind: Cluster
apiVersion: kind.x-k8s.io/v1alpha4
nodes:
- role: control-plane
- role: worker
  labels:
    node-role.kubernetes.io/worker: ""
    workload: stateless
- role: worker
  labels:
    node-role.kubernetes.io/worker: ""
    workload: stateful
EOF

# 2. Launch the cluster
kind create cluster --config kind-multinode.yaml --name lab-cluster

# 3. Verify node labels
kubectl get nodes --show-labels
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
