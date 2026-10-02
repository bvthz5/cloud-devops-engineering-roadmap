# 10 - Hands-On Practice Labs

## Lab 1: Deploying a Rootless Podman Pod with Kubernetes YAML Export

### Objective
Create a multi-container Pod in Podman, expose ports, and generate a Kubernetes YAML manifest.

### Implementation
```bash
# 1. Create Pod
podman pod create --name auth-pod -p 8080:80

# 2. Add containers
podman run -d --pod auth-pod --name web nginx:alpine

# 3. Export to Kubernetes Pod definition
podman generate kube auth-pod > auth-pod.yaml

# 4. View generated YAML
cat auth-pod.yaml
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Questions](./09-Interview-QA.md) | [README](./README.md) | [11 - Multiple-Choice Assessment](./11-MCQ.md) |
