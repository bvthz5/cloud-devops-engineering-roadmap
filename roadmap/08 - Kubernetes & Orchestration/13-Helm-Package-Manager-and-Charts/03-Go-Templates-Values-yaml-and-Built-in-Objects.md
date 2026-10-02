# 03 - Go Templates, Values.yaml, and Built-in Objects

## 1. Powerful Template Pipelines

Helm uses Go text templates enhanced with the **Sprig template library** (over 100 helper functions):

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: {{ include "my-app.fullname" . }}
  labels:
    {{- include "my-app.labels" . | nindent 4 }}
spec:
  replicas: {{ .Values.replicaCount | default 2 }}
  template:
    spec:
      containers:
      - name: {{ .Chart.Name }}
        image: "{{ .Values.image.repository }}:{{ .Values.image.tag | default .Chart.AppVersion }}"
        imagePullPolicy: {{ .Values.image.pullPolicy }}
        {{- if .Values.env }}
        env:
        {{- toYaml .Values.env | nindent 8 }}
        {{- end }}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Chart Directory Structure](./02-Helm-Chart-Directory-Structure-and-Chart-yaml.md) | [README](./README.md) | [04 - Subcharts & Library Charts](./04-Subcharts-Chart-Dependencies-and-Library-Charts.md) |
