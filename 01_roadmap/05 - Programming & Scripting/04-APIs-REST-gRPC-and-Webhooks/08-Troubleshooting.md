# 08 - APIs & Webhooks: Troubleshooting Guide

## 1. Webhook Signature Verification Fails Intermittently

### The Common Bug
Parsing JSON before verifying signatures (`req.json()`) re-serializes the JSON payload with different whitespace or key ordering, altering the cryptographic hash!

### The Fix
**Always verify the signature against the RAW, unparsed request bytes:**
```python
# In Flask:
raw_body = request.get_data() # RAW BYTES!
verify_signature(raw_body, secret, signature)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
