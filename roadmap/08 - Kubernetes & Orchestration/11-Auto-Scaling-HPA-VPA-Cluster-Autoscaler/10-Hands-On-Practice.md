# 10 - Hands-On Practice Labs

## Lab: Testing HPA Under Synthetic CPU Stress

```bash
# 1. Deploy test web application with resource requests
kubectl create deployment php-apache --image=registry.k8s.io/hpa-example
kubectl set resources deployment php-apache --requests=cpu=200m --limits=cpu=500m
kubectl expose deployment php-apache --port=80

# 2. Create HPA target 50% CPU
kubectl autoscale deployment php-apache --cpu-percent=50 --min=1 --max=5

# 3. In another terminal, generate heavy load
kubectl run -i --tty load-gen --rm --image=busybox:musl --restart=Never -- /bin/sh -c "while true; do wget -q -O- http://php-apache; done"

# 4. Observe HPA scaling live
kubectl get hpa php-apache -w
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
