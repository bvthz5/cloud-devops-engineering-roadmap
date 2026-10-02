# 03 - Headless Services and Stateful DNS Discovery

## 1. What Is a Headless Service?

A **Headless Service** is created by explicitly setting `spec.clusterIP: "None"`.
When you query DNS for a Headless Service, CoreDNS does **not** return a single virtual IP; instead, it returns an `A` record for **every individual Pod IP** matching the selector, along with `SRV` records:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: redis-cluster
spec:
  clusterIP: None               # HEADLESS!
  selector:
    app: redis
  ports:
  - port: 6379
    name: redis-port
```

---

## 2. StatefulSet DNS Discovery

When paired with a StatefulSet, each Pod receives an individual, predictable DNS subdomain:
`<pod-name>.<service-name>.<namespace>.svc.cluster.local`

Example:
- `redis-0.redis-cluster.default.svc.cluster.local` ──► `10.244.1.20`
- `redis-1.redis-cluster.default.svc.cluster.local` ──► `10.244.2.14`
This is essential for clustered databases (Kafka, Cassandra, MongoDB, Elasticsearch) where nodes need to connect directly to specific master and replica peers.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Service Types](./02-Service-Types-ClusterIP-NodePort-and-LoadBalancer.md) | [README](./README.md) | [04 - CoreDNS Architecture](./04-CoreDNS-Architecture-and-Name-Resolution-Flow.md) |
