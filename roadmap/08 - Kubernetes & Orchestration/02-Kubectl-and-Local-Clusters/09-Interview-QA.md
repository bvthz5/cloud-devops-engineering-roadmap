# 09 - Interview Questions & Architectural Scenarios

### Q1: What is a Three-Way Merge Patch in `kubectl apply`?
**Answer:**
When you run `kubectl apply -f manifest.yaml`, Kubernetes calculates the patch by comparing three states:
1. **The Original Manifest (Last-Applied-Configuration):** Stored in the resource's `metadata.annotations[kubectl.kubernetes.io/last-applied-configuration]`.
2. **The Current Live State:** The state currently stored in `etcd`.
3. **The New Submitted Manifest:** The local file you are applying.
This ensures that changes made by external controllers (like autoscalers modifying `spec.replicas`) are not accidentally overwritten when an engineer applies changes to other fields.

---

### Q2: How does Kubeadm join a worker node securely?
**Answer:**
Kubeadm uses a bootstrap token with a bidirectional verification mechanism:
1. The joining node checks the master's CA public key fingerprint using `--discovery-token-ca-cert-hash` to prevent Man-In-The-Middle (MITM) attacks.
2. The master validates the joining node's bootstrap token (`--token`).
3. Once mutually authenticated, kubelet requests a client certificate via a Certificate Signing Request (CSR), which the master automatically approves.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
