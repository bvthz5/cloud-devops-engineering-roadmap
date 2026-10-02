# 05 - Admission Controllers: Mutating and Validating Webhooks

## 1. How Webhooks Intercept Requests

Admission webhooks are HTTP callbacks that intercept API server requests:
1. **MutatingWebhookConfiguration:** Modifies requests before schema validation (e.g. auto-injecting Istio sidecars, adding mandatory resource requests).
2. **ValidatingWebhookConfiguration:** Enforces security guardrails (e.g. rejecting images with the `:latest` tag, blocking root containers).

```text
API Request ──► Mutating Webhook (Modifies YAML) ──► Schema Validation ──► Validating Webhook (Accept/Reject)
```

> [!WARNING]
> Always set `failurePolicy: Ignore` for non-critical webhooks, or ensure the webhook deployment runs with multiple replicas across distinct nodes. If a webhook with `failurePolicy: Fail` goes down, **no one can deploy anything to the cluster!**

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Pod Security Admission](./04-Pod-Security-Admission-PSA-Privileged-Baseline-Restricted.md) | [README](./README.md) | [06 - Policy as Code](./06-Policy-as-Code-with-Kyverno-and-OPA-Gatekeeper.md) |
