# 05 - API Authentication: OAuth2, JWT, and mTLS

## 1. Machine-to-Machine Authentication Patterns

In automated DevOps systems (scripts, CI runners, Kubernetes operators), three primary authentication patterns are used:

| Pattern | Mechanism | Pros | Cons |
|---|---|---|---|
| **API Keys** | Static token in header (`X-API-Key`) | Simple to implement | Revocation requires re-deploying; easy to leak |
| **OAuth2 Client Credentials** | Client ID + Client Secret exchange for temporary **JWT access token** (1hr lifespan) | Secure, short-lived tokens, centralized identity | Requires token caching and refresh logic in scripts |
| **Mutual TLS (mTLS)** | Both client and server present X.509 certificates during TLS handshake | Extreme cryptographic security; zero passwords | Certificate rotation and PKI management complexity |

---

## 2. OAuth2 Client Credentials Flow in Python

```python
import requests
import time

class OAuth2Client:
    def __init__(self, token_url: str, client_id: str, client_secret: str):
        self.token_url = token_url
        self.client_id = client_id
        self.client_secret = client_secret
        self.token = ""
        self.expires_at = 0

    def get_token(self) -> str:
        if time.time() < self.expires_at - 60: # 60s buffer
            return self.token

        resp = requests.post(
            self.token_url,
            data={"grant_type": "client_credentials"},
            auth=(self.client_id, self.client_secret),
            timeout=5
        )
        resp.raise_for_status()
        data = resp.json()
        self.token = data["access_token"]
        self.expires_at = time.time() + data.get("expires_in", 3600)
        return self.token
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Webhook Security](./04-Webhook-Security-and-HMAC-SHA256-Signatures.md) | [README](./README.md) | [06 - Rate Limiting & Jitter](./06-Rate-Limiting-Exponential-Backoff-and-Jitter.md) |
