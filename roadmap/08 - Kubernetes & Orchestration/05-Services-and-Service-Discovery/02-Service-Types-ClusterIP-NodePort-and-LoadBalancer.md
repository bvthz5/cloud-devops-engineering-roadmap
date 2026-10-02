# 02 - Service Types: ClusterIP, NodePort, and LoadBalancer

## 1. Comparison of Service Types

```text
ClusterIP (Default): Internal only
[ Pod ] ──► [ ClusterIP: 10.96.0.10 ] ──► [ Backend Pods ]

NodePort: External port opened on every node
[ External Client ] ──► [ Any Node IP:31234 ] ──► [ ClusterIP ] ──► [ Backend Pods ]

LoadBalancer: Cloud provider provisions external L4 LB
[ Internet Client ] ──► [ AWS NLB / Azure LB ] ──► [ NodePort ] ──► [ Backend Pods ]
```

---

## 2. Service Definitions

```yaml
# 1. ClusterIP (Internal Microservices)
apiVersion: v1
kind: Service
metadata:
  name: auth-service
spec:
  type: ClusterIP
  selector:
    app: auth
  ports:
  - port: 80            # Service port seen by other pods
    targetPort: 8080    # Application container listening port

---
# 2. NodePort (Exposing on Node Port Range 30000-32767)
apiVersion: v1
kind: Service
metadata:
  name: monitoring-nodeport
spec:
  type: NodePort
  selector:
    app: grafana
  ports:
  - port: 3000
    targetPort: 3000
    nodePort: 32000

---
# 3. LoadBalancer (Cloud Managed)
apiVersion: v1
kind: Service
metadata:
  name: public-api
spec:
  type: LoadBalancer
  selector:
    app: api
  ports:
  - port: 443
    targetPort: 8443
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 01 - Kubernetes Service Abstraction and Virtual IPs](./01-Kubernetes-Service-Abstraction-and-Virtual-IPs.md) | [Index](../../../README.md) | [03 - Headless Services and Stateful DNS Discovery →](./03-Headless-Services-and-Stateful-DNS-Discovery.md) |
