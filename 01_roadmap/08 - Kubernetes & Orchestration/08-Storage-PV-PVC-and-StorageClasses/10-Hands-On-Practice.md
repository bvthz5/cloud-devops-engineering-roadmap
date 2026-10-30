# 10 - Hands-On Practice Labs

## Lab: Dynamic Storage Provisioning and Verification

```bash
# 1. Create a PVC requesting dynamic storage
cat << 'EOF' | kubectl apply -f -
apiVersion: v1
kind: PersistentVolumeClaim
metadata:
  name: lab-pvc
spec:
  accessModes:
    - ReadWriteOnce
  resources:
    requests:
      storage: 1Gi
EOF

# 2. Deploy a pod mounting the volume
cat << 'EOF' | kubectl apply -f -
apiVersion: v1
kind: Pod
metadata:
  name: storage-pod
spec:
  containers:
  - name: writer
    image: busybox:musl
    command: ["sh", "-c", "echo 'PERSISTENT DATA' > /mnt/data/file.txt; sleep 3600"]
    volumeMounts:
    - name: data-vol
      mountPath: /mnt/data
  volumes:
  - name: data-vol
    persistentVolumeClaim:
      claimName: lab-pvc
EOF

# 3. Verify data persists across pod restart
kubectl exec storage-pod -- cat /mnt/data/file.txt
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
