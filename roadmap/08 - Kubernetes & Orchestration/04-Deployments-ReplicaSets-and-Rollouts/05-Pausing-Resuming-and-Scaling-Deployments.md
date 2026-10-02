# 05 - Pausing, Resuming, and Scaling Deployments

## 1. Pausing Deployments for Batch Updates

When making multiple modifications to a Deployment (e.g., updating CPU limits, adding an environment variable, changing image tag), updating them sequentially triggers multiple unnecessary rollouts. Pausing prevents intermediate rollouts:

```bash
# 1. Pause rollout
kubectl rollout pause deployment/web-api

# 2. Make multiple changes
kubectl set resources deployment/web-api -c=web --limits=cpu=500m,memory=512Mi
kubectl set env deployment/web-api FEATURE_FLAG=true
kubectl set image deployment/web-api web=my-org/api:v2.0.0

# 3. Resume rollout (triggers only ONE single rollout!)
kubectl rollout resume deployment/web-api
```

---

## 2. Declarative Scaling

```bash
# Imperative scaling
kubectl scale deployment/web-api --replicas=8
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Blue-Green & Canary](./04-Blue-Green-and-Canary-Deployments-Native-Patterns.md) | [README](./README.md) | [06 - Lifecycle Hooks](./06-Container-Lifecycle-Hooks-PostStart-and-PreStop.md) |
