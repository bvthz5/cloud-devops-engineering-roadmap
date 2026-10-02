# 10 - Hands-On Practice Labs

## Lab: Live Triage with Ephemeral Containers

```bash
# 1. Run a minimal distroless pod (no bash, no curl)
kubectl run distroless-test --image=gcr.io/distroless/static-debian11 --command -- sleep 3600

# 2. Verify you cannot exec into it
kubectl exec -it distroless-test -- sh
# Output: OCI runtime exec failed: exec: "sh": executable file not found

# 3. Attach ephemeral debug container
kubectl debug -it distroless-test --image=busybox:musl --target=distroless-test

# 4. Inside the debug container:
# You can view the target container processes via ps!
ps aux
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview QA](./09-Interview-QA.md) | [README](./README.md) | [11 - MCQ](./11-MCQ.md) |
