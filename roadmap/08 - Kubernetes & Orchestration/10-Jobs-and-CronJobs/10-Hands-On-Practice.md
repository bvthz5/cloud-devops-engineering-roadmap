# 10 - Hands-On Practice Labs

## Lab: Running a Resilient Self-Cleaning Batch Job

```yaml
cat << 'EOF' > batch-job.yaml
apiVersion: batch/v1
kind: Job
metadata:
  name: checksum-validator
spec:
  completions: 3
  parallelism: 1
  backoffLimit: 2
  ttlSecondsAfterFinished: 60
  template:
    spec:
      restartPolicy: OnFailure
      containers:
      - name: validator
        image: busybox:musl
        command: ["sh", "-c", "echo 'Checksum verified successfully'; sleep 3; exit 0"]
EOF

kubectl apply -f batch-job.yaml
kubectl get jobs -w
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview QA](./09-Interview-QA.md) | [README](./README.md) | [11 - MCQ](./11-MCQ.md) |
