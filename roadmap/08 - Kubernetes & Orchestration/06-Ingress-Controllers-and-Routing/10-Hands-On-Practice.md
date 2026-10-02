# 10 - Hands-On Practice Labs

## Lab: Path-Based Routing with Ingress-Nginx

```bash
# 1. Deploy two backend services
kubectl create deployment web1 --image=hashicorp/http-echo --port=5678 -- -text="Web 1 Response"
kubectl expose deployment web1 --port=80 --target-port=5678

kubectl create deployment web2 --image=hashicorp/http-echo --port=5678 -- -text="Web 2 Response"
kubectl expose deployment web2 --port=80 --target-port=5678

# 2. Deploy Ingress manifest
cat << 'EOF' > test-ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: lab-ingress
spec:
  ingressClassName: nginx
  rules:
  - host: lab.local
    http:
      paths:
      - path: /apple
        pathType: Prefix
        backend:
          service:
            name: web1
            port:
              number: 80
      - path: /banana
        pathType: Prefix
        backend:
          service:
            name: web2
            port:
              number: 80
EOF

kubectl apply -f test-ingress.yaml

# 3. Test with curl
curl -H "Host: lab.local" http://localhost/apple
curl -H "Host: lab.local" http://localhost/banana
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview QA](./09-Interview-QA.md) | [README](./README.md) | [11 - MCQ](./11-MCQ.md) |
