# 04 - Stick-Tables and Advanced Rate Limiting

## 1. What are Stick-Tables?

**Stick-tables** are ultra-high-speed, in-memory key-value stores built directly into HAProxy. They can track client IP addresses, session cookies, TLS session IDs, or arbitrary HTTP headers in real-time with sub-microsecond access times.

Stick-tables are used for:
1. **Session Persistence**: Pinning a user to a specific backend server.
2. **Abuse & Rate Limiting**: Tracking request rates over sliding time windows (e.g., requests per 10 seconds).
3. **Bot / DDoS Mitigation**: Tracking HTTP 4xx error rates to automatically ban brute-force scanners.

---

## 2. Production DDoS Rate Limiter Configuration

```haproxy
frontend fe_web
    bind *:443 ssl crt /etc/haproxy/certs/site.pem
    mode http

    # Define stick-table: 
    # Key: Client IP (type ip)
    # Size: 1 million IPs (size 1m)
    # Expiration: 10 minutes of inactivity (expire 10m)
    # Metrics: Request rate over last 10s (http_req_rate(10s)), Error rate (http_err_rate(10s))
    stick-table type ip size 1m expire 10m store http_req_rate(10s),http_err_rate(10s)

    # Track incoming IP in the stick-table
    http-request track-sc0 src

    # Define abuse conditions:
    # 1. More than 100 requests in 10 seconds (10 req/s average)
    acl is_flooding sc_http_req_rate(0) gt 100
    # 2. More than 20 HTTP 4xx/5xx errors in 10 seconds (Brute force scanner)
    acl is_bruteforce sc_http_err_rate(0) gt 20

    # Deny and return HTTP 429 Too Many Requests
    http-request deny deny_status 429 if is_flooding
    http-request deny deny_status 403 if is_bruteforce

    default_backend be_app
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Layer 4 vs Layer 7 Proxying and ACLs](./03-Layer-4-vs-Layer-7-Proxying-and-ACLs.md) | [Index](../../../README.md) | [05 - Active Health Checks and Graceful Failover →](./05-Active-Health-Checks-and-Graceful-Failover.md) |
