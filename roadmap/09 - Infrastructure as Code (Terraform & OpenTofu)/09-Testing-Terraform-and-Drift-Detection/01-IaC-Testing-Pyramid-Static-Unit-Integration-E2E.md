# 01 - IaC Testing Pyramid

```text
        /\
       /  \        E2E Tests (Full stack deploy + verify)
      /    \       Slow, expensive, high confidence
     /------\
    /        \     Integration Tests (Deploy module + verify)
   /          \    Medium speed, real resources
  /------------\
 /              \  Unit Tests (terraform test, plan assertions)
/                \ Fast, no real resources
/------------------\
Static Analysis       tflint, fmt, validate, checkov, tfsec
  Fastest, zero cost
```

| Layer | Tool | Speed | Cost | Confidence |
|---|---|---|---|---|
| Static Analysis | tflint, fmt, validate | Instant | Free | Low |
| Unit Test | terraform test (plan mode) | Fast | Free | Medium |
| Integration Test | terraform test (apply mode), Terratest | Slow | Cloud costs | High |
| E2E Test | Full stack deploy + smoke tests | Slowest | High cloud costs | Highest |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [README](./README.md) | [README](./README.md) | [02 - Static Analysis](./02-Static-Analysis-tflint-terraform-validate-fmt.md) |
