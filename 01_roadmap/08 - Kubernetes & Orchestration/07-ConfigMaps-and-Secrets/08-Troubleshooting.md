# 08 - Troubleshooting & Diagnostic Runbooks

## Diagnostic Decision Tree: ConfigMap & Secret Issues

```text
[ Symptom: Pod fails to start with "CreateContainerConfigError" ]
                         │
                         ▼
        Run: kubectl describe pod <pod-name>
        Look at "Events" section at bottom:
        ├── "configmap 'app-config' not found" ──► ConfigMap does not exist in this namespace!
        ├── "secret 'db-pass' not found"       ──► Secret is missing or named incorrectly.
        └── "key 'PORT' not found"             ──► Key is misspelled inside the ConfigMap.

[ Symptom: Pod mounted volume file is empty or permission denied ]
                         │
                         ▼
        Check defaultMode in volume specification:
        volumes:
        - name: cfg
          configMap:
            name: my-cfg
            defaultMode: 0644  (Ensure non-root container user can read the file!)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
