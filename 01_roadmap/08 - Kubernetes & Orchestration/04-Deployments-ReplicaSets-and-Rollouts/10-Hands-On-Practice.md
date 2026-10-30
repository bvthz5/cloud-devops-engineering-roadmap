# 10 - Hands-On Practice Labs

## Lab: Zero-Downtime Rolling Update & Rollback Verification

```bash
# 1. Create a deployment with version 1
kubectl create deployment web-demo --image=nginx:1.24 --replicas=4

# 2. Configure rolling update strategy
kubectl patch deployment web-demo -p '{"spec":{"strategy":{"rollingUpdate":{"maxSurge":1,"maxUnavailable":0}}}}'

# 3. Trigger rolling update to nginx 1.25
kubectl set image deployment/web-demo nginx=nginx:1.25

# 4. Monitor rollout progression
kubectl rollout status deployment/web-demo

# 5. Undo the rollout immediately
kubectl rollout undo deployment/web-demo
kubectl rollout status deployment/web-demo
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
