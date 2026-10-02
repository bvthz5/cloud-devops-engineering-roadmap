# 10 - Hands-On Practice Labs

## Lab: Enforcing Zero-Trust Isolation with NetworkPolicies

```bash
# 1. Create two test namespaces
kubectl create ns frontend-tier
kubectl create ns backend-tier

# 2. Deploy backend service
kubectl run database --namespace=backend-tier --image=redis:alpine --labels=app=redis
kubectl expose pod database --namespace=backend-tier --port=6379

# 3. Apply strict network policy: Only pods with label tier=web can hit redis
cat << 'EOF' | kubectl apply -f -
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: isolate-redis
  namespace: backend-tier
spec:
  podSelector:
    matchLabels:
      app: redis
  policyTypes:
  - Ingress
  ingress:
  - from:
    - namespaceSelector:
        matchLabels:
          kubernetes.io/metadata.name: frontend-tier
      podSelector:
        matchLabels:
          tier: web
    ports:
    - protocol: TCP
      port: 6379
EOF

# 4. Test unauthorized access (MUST TIMEOUT!)
kubectl run test --rm -it --namespace=default --image=busybox:musl -- nc -zv database.backend-tier.svc.cluster.local 6379 -w 3
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
