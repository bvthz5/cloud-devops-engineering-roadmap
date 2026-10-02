# 04 - Subcharts, Chart Dependencies, and Library Charts

## 1. Library Charts (`type: library`)

Enterprise platforms avoid repeating boilerplate Deployment/Service YAML across 100 microservices by creating a single shared **Library Chart**.
- A Library chart only provides named template helpers (`_helpers.tpl`).
- Application charts include it as a dependency and call its templates with a single line!

---

## 2. Managing Dependencies

```bash
# Update and download chart dependencies declared in Chart.yaml
helm dependency update ./payment-service

# Build dependency archives
helm dependency build ./payment-service
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Go Templates & Values](./03-Go-Templates-Values-yaml-and-Built-in-Objects.md) | [README](./README.md) | [05 - Helm Hooks](./05-Helm-Hooks-and-Lifecycle-Management.md) |
