# Kubernetes & Orchestration Interview Scenarios: Rapid-Fire Drills & Master Cheat Sheet

> **Interview Focus:** Real-world incident response, root-cause analysis (RCA), diagnostic toolchains, and articulate candidate verbal pitches.

---

## 📌 Scenario 10: Kubernetes Secret Accidental Exposure in Base64 Manifest

### 🚨 The Production Scenario
A security scan reveals that database passwords stored in Kubernetes `Secret` manifests committed to a Git repository can be decoded instantly by any developer with read access.

### 🔬 Root Cause Analysis (RCA) & Deep Mechanics
Standard Kubernetes Secrets are merely Base64-encoded strings, NOT cryptographically encrypted. Base64 is an encoding format designed for safe binary-to-text transmission, offering zero confidentiality. Anyone with access to the YAML can decode the secret with `echo '<base64>' | base64 -d`.

### 🛠️ Step-by-Step Triage & Troubleshooting Workflow
- Step 1: Revoke all exposed credentials immediately in the database/IAM provider.
- Step 2: Implement an automated secret management solution (External Secrets Operator with AWS Secrets Manager or HashiCorp Vault).
- Step 3: For GitOps workflows, adopt asymmetric encryption tooling like Bitnami `SealedSecrets` or Mozilla `SOPS`.
- Step 4: Enable Kubernetes KMS encryption at rest in `kube-apiserver` (`--encryption-provider-config`).
- Step 5: Enforce strict RBAC preventing unauthorized users from running `kubectl get secret -o yaml`.

### 💻 Exact Diagnostic & Remediation Commands
```bash
# Demonstrate how trivially base64-encoded Kubernetes secrets are decoded
echo '<BASE64_STRING>' | base64 --decode

# Extract and decode secret payload via kubectl
kubectl get secret my-secret -o jsonpath='{.data.password}' | base64 -d

# Install External Secrets Operator to pull secrets directly from Vault/AWS Secrets Manager
helm repo add external-secrets https://charts.external-secrets.io

# Encrypt secret with cluster public key so it can be safely committed to Git
kubeseal --format=yaml < secret.yaml > sealedsecret.yaml

# Generate secret manifest without committing plaintext files to disk
kubectl create secret generic db-creds --from-literal=password=$DB_PASS --dry-run=client -o yaml

```

### 🗣️ Ideal Interview Candidate Answer (Verbal Pitch)
> "Kubernetes Secrets are Base64 encoded, which provides zero security. In enterprise architectures, secrets must never be committed to Git in raw format. I implement the External Secrets Operator (ESO) paired with AWS Secrets Manager or HashiCorp Vault: Kubernetes dynamically synchronizes secrets directly from Vault into cluster memory. In etcd, I enable envelope encryption at rest via AWS KMS or HashiCorp Vault KMS plugins."

---

## ⚡ Rapid-Fire Diagnostic Cheat Sheet

| Symptom / Scenario | First Command to Run | Root Cause Hypothesis |
| :--- | :--- | :--- |
| **Scenario 1: Production Pods Stuck in 'Pending' State during Traffic Surge** | `kubectl describe pod <PENDING_POD> \| grep -A 10 'Events:'` | The Kubernetes `kube-scheduler` cannot schedule the pods onto any available node... |
| **Scenario 2: HTTP 502 Bad Gateway during Rolling Update of Deployment** | `kubectl rollout status deployment/my-api` | There are two distinct race conditions during pod termination and startup: 1) Th... |
| **Scenario 3: Kubernetes Worker Node Flapping between 'Ready' and 'NotReady'** | `kubectl cordon <NODE_NAME>` | The Kubernetes node controller marks a node `NotReady` when `kubelet` fails to p... |
| **Scenario 4: StatefulSet Pod Stuck in 'Terminating' State during Node Failure** | `kubectl get pod -l app=mongodb -o wide` | Unlike Deployments (which are stateless), StatefulSets enforce at-most-one seman... |
| **Scenario 5: CoreDNS Pods Crashing or CPU Throttled under Cluster Load** | `kubectl top pod -n kube-system -l k8s-app=kube-dns` | All Kubernetes DNS traffic routes through the CoreDNS deployment in `kube-system... |
| **Scenario 6: Silent CPU Throttling on Microservices Despite Low CPU Usage** | `kubectl exec <POD> -- cat /sys/fs/cgroup/cpu/cpu.stat` | Kubernetes enforces CPU limits using Linux Completely Fair Scheduler (CFS) bandw... |
| **Scenario 7: Cluster Out of IP Addresses (CNI IPAM Subnet Exhaustion)** | `aws ec2 describe-subnets --subnet-ids <SUBNET_ID> --query 'Subnets[*].AvailableIpAddressCount'` | Cloud-native CNIs (like AWS VPC CNI or Azure CNI) assign secondary private IP ad... |
| **Scenario 8: Ingress Controller Returning 503 Service Temporarily Unavailable** | `kubectl logs -n ingress-nginx -l app.kubernetes.io/name=ingress-nginx --tail=100 \| grep '503'` | When an Ingress controller routes traffic directly to Pod IPs, it synchronizes i... |
| **Scenario 9: RBAC Authorization Denial Blocking Microservices from Kubernetes API** | `kubectl auth can-i list pods --as=system:serviceaccount:monitoring:prometheus-sa` | Kubernetes enforces strict Role-Based Access Control (RBAC). By default, pods mo... |
| **Scenario 10: Kubernetes Secret Accidental Exposure in Base64 Manifest** | `echo '<BASE64_STRING>' \| base64 --decode` | Standard Kubernetes Secrets are merely Base64-encoded strings, NOT cryptographic... |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Kubernetes & Orchestration Interview Scenarios: Security & Disaster Recovery Scenarios](03-Security-and-Disaster-Recovery-Scenarios.md) | [Index](../../../README.md) | [Infrastructure as Code (Terraform & OpenTofu) Interview Scenarios: Production Incidents & Triage Scenarios →](../06-Infrastructure-as-Code-Terraform-and-OpenTofu-Scenarios/01-Production-Incidents-and-Triage-Scenarios.md) |

