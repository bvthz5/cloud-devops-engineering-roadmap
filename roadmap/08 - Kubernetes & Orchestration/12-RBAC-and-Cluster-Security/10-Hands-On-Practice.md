# 10 - Hands-On Practice Labs

## Lab: Enforcing Pod Security Standards (Restricted)

```bash
# 1. Create a namespace with strict Pod Security Admission
kubectl create namespace restricted-lab
kubectl label namespace restricted-lab pod-security.kubernetes.io/enforce=restricted

# 2. Attempt to run a privileged pod (MUST FAIL!)
kubectl run privileged-pod --namespace=restricted-lab --image=nginx --privileged
# Output: Error from server (Forbidden): violates PodSecurity "restricted:latest"

# 3. Deploy compliant hardened pod
cat << 'EOF' | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: secure-nginx
  namespace: restricted-lab
spec:
  securityContext:
    runAsNonRoot: true
    runAsUser: 10001
    seccompProfile:
      type: RuntimeDefault
  containers:
  - name: nginx
    image: nginxinc/nginx-unprivileged:alpine
    securityContext:
      allowPrivilegeEscalation: false
      capabilities:
        drop: ["ALL"]
EOF
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
