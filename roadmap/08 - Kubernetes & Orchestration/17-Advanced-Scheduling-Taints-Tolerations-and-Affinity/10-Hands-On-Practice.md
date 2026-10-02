# 10 - Hands-On Practice Labs

## Lab: Tainting a Node for Dedicated Workloads

```bash
# 1. Apply a taint to a worker node
kubectl taint nodes worker-01 dedicated=analytics:NoSchedule

# 2. Deploy standard pod (will fail to schedule on worker-01)
kubectl run standard-app --image=nginx:alpine

# 3. Deploy pod with matching toleration
cat << 'EOF' | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: analytics-pod
spec:
  tolerations:
  - key: "dedicated"
    operator: "Equal"
    value: "analytics"
    effect: "NoSchedule"
  containers:
  - name: worker
    image: busybox:musl
    command: ["sleep", "3600"]
EOF

# 4. Verify analytics-pod is scheduled on worker-01
kubectl get pod analytics-pod -o wide
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview QA](./09-Interview-QA.md) | [README](./README.md) | [11 - MCQ](./11-MCQ.md) |
