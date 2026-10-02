# 10 - Hands-On Practice Labs

## Lab: Deploying a Resilient Pod with Probes, Init Container, and Resource Boundaries

```yaml
cat << 'EOF' > resilient-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: resilient-service
  labels:
    app: api
spec:
  initContainers:
  - name: config-init
    image: busybox:musl
    command: ['sh', '-c', 'echo "INITIALIZED" > /workdir/status.txt']
    volumeMounts:
    - name: shared-data
      mountPath: /workdir
  containers:
  - name: web
    image: nginx:alpine
    resources:
      requests:
        cpu: "100m"
        memory: "64Mi"
      limits:
        cpu: "250m"
        memory: "128Mi"
    livenessProbe:
      httpGet:
        path: /
        port: 80
      initialDelaySeconds: 5
      periodSeconds: 10
    volumeMounts:
    - name: shared-data
      mountPath: /usr/share/nginx/html
  volumes:
  - name: shared-data
    emptyDir: {}
EOF

kubectl apply -f resilient-pod.yaml
kubectl get pod resilient-service -w
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
