# 06 - Production Bootstrap with Kubeadm

## 1. The Production Kubeadm Workflow

`kubeadm` is the official standard toolkit for bootstrapping production Kubernetes clusters from scratch.

```text
[ Pre-Flight Checks (swap disabled, cgroups v2, containerd) ]
                           │
                           ▼
[ Control Plane Init: kubeadm init --pod-network-cidr=192.168.0.0/16 ]
   ├── Generates self-signed CA and PKI certs in /etc/kubernetes/pki/
   ├── Writes kubeconfig files in /etc/kubernetes/
   ├── Generates static pod manifests in /etc/kubernetes/manifests/
   └── Generates bootstrap token for worker nodes
                           │
                           ▼
[ Install CNI (e.g., Calico or Cilium) ]
   └── Worker nodes transition from NotReady ──► Ready
                           │
                           ▼
[ Join Workers: kubeadm join <master-ip>:6443 --token ... --discovery-token-ca-cert-hash ... ]
```

---

## 2. Kubeadm Configuration File (`kubeadm-config.yaml`)

```yaml
apiVersion: kubeadm.k8s.io/v1beta3
kind: ClusterConfiguration
kubernetesVersion: v1.30.0
controlPlaneEndpoint: "k8s-vip.internal:6443"
networking:
  podSubnet: "10.244.0.0/16"
  serviceSubnet: "10.96.0.0/12"
---
apiVersion: kubelet.config.k8s.io/v1beta1
kind: KubeletConfiguration
cgroupDriver: systemd
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - Lightweight Edge Kubernetes with K3s and MicroK8s](./05-Lightweight-Edge-Kubernetes-with-K3s-and-MicroK8s.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
