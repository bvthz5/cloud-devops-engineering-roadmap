# 03 - Init Containers, Sidecars, and Ephemeral Containers

## 1. Init Containers (Sequential Bootstrapping)

Init containers run **to completion sequentially** before application containers start. If an init container fails, the Pod restarts it until it succeeds.

```yaml
spec:
  initContainers:
  - name: wait-for-db
    image: busybox:musl
    command: ['sh', '-c', 'until nc -z postgres.default.svc.cluster.local 5432; do echo waiting for db; sleep 2; done;']
  - name: run-migrations
    image: my-org/app:v1
    command: ['python', 'manage.py', 'migrate']
  containers:
  - name: web-app
    image: my-org/app:v1
```

---

## 2. Native Sidecars (Kubernetes v1.28+)

Starting in Kubernetes 1.28+, containers inside `initContainers` can specify `restartPolicy: Always`. They start *before* primary application containers and remain running for the entire Pod lifecycle:

```yaml
spec:
  initContainers:
  - name: vault-agent-sidecar
    image: hashicorp/vault:1.15.0
    restartPolicy: Always       # NATIVE SIDECAR!
    args: ["agent", "-config=/etc/vault/vault-agent-config.hcl"]
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Health Probes](./02-Liveness-Readiness-and-Startup-Probes.md) | [README](./README.md) | [04 - QoS & Resource Limits](./04-Resource-Requests-Limits-and-Quality-of-Service-QoS.md) |
