# 10 - Hands-On Practice Labs

## Lab: Verifying CoreDNS & Service Resolution with Netshoot

```bash
# 1. Deploy test web server
kubectl create deployment web --image=nginx:alpine --replicas=2
kubectl expose deployment web --port=80 --target-port=80

# 2. Spin up interactive networking debug pod
kubectl run netshoot --rm -it --image=nicolaka/netshoot -- sh

# 3. Test DNS resolution inside the pod
dig web.default.svc.cluster.local
nslookup web

# 4. Test HTTP connection via VIP
curl -I http://web.default.svc.cluster.local
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview QA](./09-Interview-QA.md) | [README](./README.md) | [11 - MCQ](./11-MCQ.md) |
