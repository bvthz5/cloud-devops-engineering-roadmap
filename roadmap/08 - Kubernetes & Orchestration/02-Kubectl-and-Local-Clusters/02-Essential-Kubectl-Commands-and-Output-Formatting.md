# 02 - Essential Kubectl Commands and Output Formatting

## 1. Output Formatting Mastery (JSONPath & Custom Columns)

In enterprise operations, formatting `kubectl` output is essential for scripting and automation.

```bash
# 1. Custom Columns for customized reporting
kubectl get pods -o custom-columns=\
  NAME:.metadata.name,\
  NODE:.spec.nodeName,\
  IP:.status.podIP,\
  STATUS:.status.phase

# 2. Extract internal Pod IP using JSONPath
kubectl get pod web-app -o jsonpath='{.status.podIP}'

# 3. List all node internal IP addresses
kubectl get nodes -o jsonpath='{.items[*].status.addresses[?(@.type=="InternalIP")].address}'

# 4. Sort pods by restart count
kubectl get pods -A --sort-by='.status.containerStatuses[0].restartCount'
```

---

## 2. Imperative vs Declarative Management

| Action | Imperative (`kubectl create / run / scale`) | Declarative (`kubectl apply -f manifest.yaml`) |
|---|---|---|
| **Speed** | Instant one-liner execution | Requires file preparation |
| **Auditability** | None (lost in terminal history) | Full GitOps history and commit logs |
| **Conflict Handling**| Overwrites or errors out | Three-Way Merge Patch |
| **Production Fit** | Debugging / Emergency troubleshooting only | **Mandatory production standard** |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Kubectl Architecture](./01-Kubectl-Architecture-Kubeconfig-and-Contexts.md) | [README](./README.md) | [03 - Kubectl Plugins & Krew](./03-Kubectl-Plugins-and-Krew-Ecosystem.md) |
