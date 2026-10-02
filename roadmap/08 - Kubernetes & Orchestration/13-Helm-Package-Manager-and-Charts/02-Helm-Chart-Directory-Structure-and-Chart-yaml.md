# 02 - Helm Chart Directory Structure and Chart.yaml

## 1. Standard Chart Hierarchy

```text
my-microservice/
├── Chart.yaml             # Chart metadata, semantic version, and dependencies
├── values.yaml            # Default configuration values for templates
├── values.schema.json     # Optional JSON schema to strictly validate values.yaml!
├── .helmignore            # Files to ignore during packaging
├── templates/             # Kubernetes YAML template files
│   ├── deployment.yaml
│   ├── service.yaml
│   ├── ingress.yaml
│   ├── hpa.yaml
│   ├── NOTES.txt          # Displayed to user after installation
│   └── _helpers.tpl       # Named Go template definitions (partials)
└── charts/                # Directory containing subchart dependencies (.tgz)
```

---

## 2. Production `Chart.yaml`

```yaml
apiVersion: v2
name: payment-service
description: High-throughput payment processing engine
type: application
version: 1.4.0              # The version of the HELM CHART
appVersion: "2.18.3"        # The version of the INNER APPLICATION
dependencies:
- name: redis
  version: 18.0.1
  repository: https://charts.bitnami.com/bitnami
  condition: redis.enabled
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Helm v3 Architecture](./01-Helm-v3-Architecture-and-Release-Lifecycle.md) | [README](./README.md) | [03 - Go Templates & Values](./03-Go-Templates-Values-yaml-and-Built-in-Objects.md) |
