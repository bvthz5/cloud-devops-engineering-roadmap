# 04 - Webhook Security and HMAC-SHA256 Signatures

## 1. The Threat: Unauthorized Webhook Spoofing

Webhook endpoints are publicly exposed to the internet. An attacker can send fake `POST /webhook` requests declaring that a deployment passed or a payment was confirmed!

---

## 2. HMAC-SHA256 Signature Verification

To guarantee authenticity and payload integrity:
1. Senders share a **Secret Token** with the receiver.
2. The sender computes a cryptographic hash of the raw payload using HMAC-SHA256:
   $$\text{Signature} = \text{HMAC-SHA256}(\text{Raw Payload}, \text{Secret})$$
3. Sender attaches the signature to an HTTP header: `X-Hub-Signature-256: sha256=a8f2...`
4. The receiver recomputes the HMAC-SHA256 hash using the shared secret and compares using constant-time comparison (`hmac.compare_digest`).

```python
import hmac
import hashlib

def verify_github_signature(payload_bytes: bytes, secret: str, received_sig: str) -> bool:
    expected_sig = "sha256=" + hmac.new(
        secret.encode("utf-8"),
        payload_bytes,
        hashlib.sha256
    ).hexdigest()
    # Constant-time comparison prevents timing attacks!
    return hmac.compare_digest(expected_sig, received_sig)
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [03 - Webhooks](./03-Webhook-Architecture-and-Reliable-Delivery.md) | [README](./README.md) | [05 - API Authentication](./05-API-Authentication-OAuth2-JWT-and-mTLS.md) |
