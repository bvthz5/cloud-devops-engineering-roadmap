# 03 - Kubectl Plugins and the Krew Ecosystem

## 1. How Kubectl Plugin Architecture Works

`kubectl` discovers any executable file in your system `$PATH` whose filename starts with `kubectl-`:

```bash
# Create custom plugin
cat << 'EOF' > /usr/local/bin/kubectl-podips
#!/usr/bin/env bash
kubectl get pods -o custom-columns=NAME:.metadata.name,IP:.status.podIP --no-headers
EOF

chmod +x /usr/local/bin/kubectl-podips

# Execute custom plugin natively
kubectl podips
```

---

## 2. Krew: The Official Plugin Manager

**Krew** is the plugin manager for `kubectl`, standardizing plugin installation and updates.

```bash
# 1. Install critical DevOps plugins
kubectl krew install ctx       # Rapid context switching
kubectl krew install ns        # Rapid namespace switching
kubectl krew install neat      # Cleans clutter from exported manifests
kubectl krew install who-can   # RBAC permission queries

# 2. Usage examples
kubectl ctx staging-cluster
kubectl ns production
kubectl who-can delete pods -n default
kubectl get pod my-pod -o yaml | kubectl neat > clean-pod.yaml
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Essential Kubectl Commands](./02-Essential-Kubectl-Commands-and-Output-Formatting.md) | [README](./README.md) | [04 - Kind & Minikube](./04-Local-Cluster-Setup-with-Kind-and-Minikube.md) |
