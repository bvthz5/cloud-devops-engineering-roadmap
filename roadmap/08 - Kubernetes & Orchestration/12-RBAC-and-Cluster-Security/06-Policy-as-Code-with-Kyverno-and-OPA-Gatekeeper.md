# 06 - Policy as Code with Kyverno and OPA Gatekeeper

## 1. Kyverno (Kubernetes Native) vs OPA Gatekeeper

- **Kyverno:** Policies are written in standard Kubernetes YAML. Requires zero programming knowledge. Supports validation, mutation, and resource generation.
- **OPA Gatekeeper:** Uses the Open Policy Agent engine and the **Rego** declarative query language. Ideal for complex multi-platform policy enforcement across Kubernetes, Terraform, and Envoy.

---

## 2. Production Kyverno Policy: Disallow `:latest` Tag

```yaml
apiVersion: kyverno.io/v1
kind: ClusterPolicy
metadata:
  name: disallow-latest-tag
spec:
  validationFailureAction: Enforce
  rules:
  - name: validate-image-tag
    match:
      any:
      - resources:
          kinds:
          - Pod
    validate:
      message: "Using ':latest' image tag is strictly forbidden in production!"
      pattern:
        spec:
          containers:
          - image: "!*:latest"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [05 - Admission Controllers](./05-Admission-Controllers-Mutating-and-Validating-Webhooks.md) | [README](./README.md) | [07 - Real-World Scenarios](./07-Real-World-Scenarios.md) |
