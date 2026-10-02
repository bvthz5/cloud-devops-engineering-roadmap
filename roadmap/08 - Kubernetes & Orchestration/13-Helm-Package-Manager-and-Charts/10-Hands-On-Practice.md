# 10 - Hands-On Practice Labs

## Lab: Scaffolding, Customizing, and Installing a Helm Chart

```bash
# 1. Create a clean chart template
helm create lab-chart

# 2. Test rendering
helm template test lab-chart/ --set replicaCount=3

# 3. Install release
helm install lab-app ./lab-chart --set service.type=NodePort

# 4. Verify release
helm list
kubectl get pods -l app.kubernetes.io/instance=lab-app

# 5. Clean up
helm uninstall lab-app
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
