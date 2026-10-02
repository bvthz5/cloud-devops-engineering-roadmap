# 09 - APIs & Webhooks: Interview Questions & Answers

### Q1: What is the architectural difference between PUT and PATCH in REST?
**Answer:** `PUT` replaces the target resource entirely with the request payload (if fields are omitted, they are cleared or set to defaults). `PATCH` applies partial modifications to the resource, leaving omitted fields unchanged. Both should be designed idempotently where possible.

### Q2: How do you protect a webhook receiver from being flooded or spoofed by malicious third parties?
**Answer:**
1. Enforce **HMAC-SHA256 signature verification** on the raw body payload using a shared secret.
2. Verify timestamps to prevent replay attacks (reject payloads older than 5 minutes).
3. Validate sender source IP ranges against published provider CIDRs (e.g. GitHub webhook IP ranges).
4. Return `HTTP 200 OK` within milliseconds and offload processing to an asynchronous message queue.

### Q3: Why is Jitter added to Exponential Backoff in automated API clients?
**Answer:** Exponential backoff spaces out retries, but if multiple clients failed simultaneously, they will all retry at identical synchronized intervals. Adding random Jitter desynchronizes the retry requests, smoothing out traffic spikes and allowing congested servers to recover.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Troubleshooting](./08-Troubleshooting.md) | [README](./README.md) | [10 - Hands-On Practice](./10-Hands-On-Practice.md) |
