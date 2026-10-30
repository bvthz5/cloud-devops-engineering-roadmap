# 01 - Kubectl Architecture, Kubeconfig, and Contexts

## 1. How Kubectl Works Under the Hood

`kubectl` is a command-line client that translates user commands into RESTful HTTP/JSON requests sent to the Kubernetes `kube-apiserver`.

```text
~/.kube/config
   ├── Clusters  (API server URL, CA cert)
   ├── Users     (Client certs, IAM tokens)
   └── Contexts  (Binding: Cluster + User + Default Namespace)
         │
         ▼
kubectl get pods -n production
   │ (Reads active context)
   ▼
Constructs REST Request:
GET https://k8s-api.example.com:6443/api/v1/namespaces/production/pods
Header: Authorization: Bearer <token>
```

---

## 2. Anatomy of `~/.kube/config`

```yaml
apiVersion: v1
kind: Config
preferences: {}

clusters:
- cluster:
    certificate-authority-data: LS0tLS1CRUdJTi...
    server: https://192.168.1.100:6443
  name: production-cluster

users:
- name: devops-admin
  user:
    client-certificate-data: LS0tLS1CRUdJTi...
    client-key-data: LS0tLS1CRUdJTi...

contexts:
- context:
    cluster: production-cluster
    namespace: payments
    user: devops-admin
  name: prod-payments-ctx

current-context: prod-payments-ctx
```

---

## 3. Managing Contexts Without Errors

```bash
# View all available contexts
kubectl config get-contexts

# Switch active context immediately
kubectl config use-context prod-payments-ctx

# Permanently set default namespace for current context
kubectl config set-context --current --namespace=monitoring
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (01-Kubernetes-Architecture-and-Control-Plane)](../01-Kubernetes-Architecture-and-Control-Plane/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Essential Kubectl Commands and Output Formatting →](./02-Essential-Kubectl-Commands-and-Output-Formatting.md) |
