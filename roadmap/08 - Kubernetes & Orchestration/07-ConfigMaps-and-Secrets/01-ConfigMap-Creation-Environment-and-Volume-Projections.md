# 01 - ConfigMap Creation, Environment, and Volume Projections

## 1. What Is a ConfigMap?

A **ConfigMap** is an API object used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as:
1. Environment variables (individual keys or bulk `envFrom`).
2. Command-line arguments in `spec.containers[*].args`.
3. Read-only configuration files mounted via a Volume.

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: app-config
data:
  APP_ENV: "production"
  LOG_LEVEL: "info"
  nginx.conf: |
    server {
      listen 80;
      location / {
        return 200 'healthy';
      }
    }
```

---

## 2. Consuming ConfigMaps in a Pod

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: demo-pod
spec:
  containers:
  - name: app
    image: nginx:alpine
    # 1. Injected as Environment Variables
    env:
    - name: RUNTIME_ENV
      valueFrom:
        configMapKeyRef:
          name: app-config
          key: APP_ENV
    # 2. Mounted as Configuration File
    volumeMounts:
    - name: config-volume
      mountPath: /etc/nginx/conf.d
  volumes:
  - name: config-volume
    configMap:
      name: app-config
      items:
      - key: nginx.conf
        path: default.conf
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (06-Ingress-Controllers-and-Routing)](../06-Ingress-Controllers-and-Routing/12-Quick-Revision.md) | [Index](../../../README.md) | [02 - Kubernetes Secrets Types and Base64 Encoding Realities →](./02-Kubernetes-Secrets-Types-and-Base64-Encoding-Realities.md) |
