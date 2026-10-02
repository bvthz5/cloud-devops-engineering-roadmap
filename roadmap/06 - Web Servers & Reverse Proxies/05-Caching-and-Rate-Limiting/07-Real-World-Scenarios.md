# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: Private User Data Cache Leak Outage

### Context & Incident
A FinTech company enabled Nginx proxy caching on their `/api/account/summary` endpoint to reduce database load. Minutes after deployment, user Alice posted a screenshot showing that she was seeing user Bob's bank account balance and transaction history!

### Root Cause
The default `proxy_cache_key` was:
```nginx
proxy_cache_key "$scheme$request_method$host$request_uri";
```
Because `/api/account/summary` took the user's identity from the `Authorization: Bearer <token>` header and had identical URL paths for all users, Nginx cached Bob's response and served it to Alice from the cache!

### Architectural Solution
1. **Never cache authenticated endpoints**: Enforce `Cache-Control: private, no-store` on all personalized data.
2. In Nginx:
   ```nginx
   # Bypass cache for any request with Authorization or Cookie header
   proxy_cache_bypass $http_authorization $http_cookie;
   proxy_no_cache $http_authorization $http_cookie;
   ```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - DDoS Mitigation & Security Throttling](./06-DDoS-Mitigation-and-IP-Reputation-Filtering.md) | [README](./README.md) | [08 - Troubleshooting](./08-Troubleshooting.md) |
