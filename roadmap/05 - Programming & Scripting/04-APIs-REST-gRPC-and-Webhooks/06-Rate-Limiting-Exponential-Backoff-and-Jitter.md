# 06 - Rate Limiting, Exponential Backoff, and Jitter

## 1. The Thundering Herd Problem

When a central API (e.g. AWS or GitHub) recovers from an outage, thousands of automated scripts retry at the exact same interval (e.g. every 5 seconds). The synchronized flood of retries immediately crashes the recovering API!

---

## 2. Exponential Backoff with Full Jitter

To prevent synchronized retry stampedes, Amazon Architecture recommends **Full Jitter**:

$$t = \text{random}(0, \min(\text{max\_wait}, \text{base} \times 2^{\text{attempt}}))$$

```python
import random
import time

def retry_with_jitter(max_attempts=5, base_delay=1.0, max_delay=30.0):
    for attempt in range(max_attempts):
        try:
            return call_api()
        except requests.HTTPError as e:
            if e.response.status_code == 429 or e.response.status_code >= 500:
                # Exponential backoff with Full Jitter
                cap = min(max_delay, base_delay * (2 ** attempt))
                sleep_time = random.uniform(0, cap)
                time.sleep(sleep_time)
            else:
                raise
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 05 - API Authentication OAuth2 JWT and mTLS](./05-API-Authentication-OAuth2-JWT-and-mTLS.md) | [Index](../../../README.md) | [07 - Real World Scenarios →](./07-Real-World-Scenarios.md) |
