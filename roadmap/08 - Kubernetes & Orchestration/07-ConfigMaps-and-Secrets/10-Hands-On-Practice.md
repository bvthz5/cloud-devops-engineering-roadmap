# 10 - Hands-On Practice Labs

## Lab: Testing Atomic Symlink Volume Updates

```bash
# 1. Create initial ConfigMap
kubectl create configmap dynamic-msg --from-literal=message="Version 1"

# 2. Run pod mounting the ConfigMap
cat << 'EOF' | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: config-watcher
spec:
  containers:
  - name: watcher
    image: busybox:musl
    command: ["sh", "-c", "while true; do cat /config/message; sleep 2; done"]
    volumeMounts:
    - name: cfg-vol
      mountPath: /config
  volumes:
  - name: cfg-vol
    configMap:
      name: dynamic-msg
EOF

# 3. View live output
kubectl logs -f config-watcher &

# 4. Update ConfigMap live
kubectl create configmap dynamic-msg --from-literal=message="Version 2 UPDATED" --dry-run=client -o yaml | kubectl apply -f -

# 5. Observe output change within 30-60 seconds without pod restart!
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
