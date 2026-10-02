# 09 - Interview Questions & Architectural Scenarios

### Q1: What is the difference between `nodeSelector` and `nodeAffinity`?
**Answer:**
`nodeSelector` is the simplest mechanism; it only supports strict equality matching (`key: value`).
`nodeAffinity` is much more expressive: it supports operators (`In`, `NotIn`, `Exists`, `DoesNotExist`, `Gt`, `Lt`) and allows both hard requirements (`requiredDuringScheduling...`) and soft preferences (`preferredDuringScheduling...`) with customizable scoring weights.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
