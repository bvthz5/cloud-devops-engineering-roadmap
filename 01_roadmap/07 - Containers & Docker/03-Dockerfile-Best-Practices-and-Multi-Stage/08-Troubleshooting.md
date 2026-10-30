# 08 - Troubleshooting & Diagnostic Runbooks

## 1. Inspecting Image Layers with `dive`

`dive` is a terminal UI tool that analyzes Docker image layer contents and identifies wasted space:

```bash
# Install and inspect image
dive myregistry.com/app:v1.0
```

It highlights:
- Wasted space (duplicated files across layers).
- Inefficient caching.
- Exact file additions per layer.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
