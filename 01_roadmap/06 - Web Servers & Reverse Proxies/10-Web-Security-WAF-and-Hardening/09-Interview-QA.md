# 09 - Interview Questions & Architectural Scenarios

### Q1: Explain how Anomaly Scoring works in the OWASP Core Rule Set (CRS).
**Answer**: In anomaly scoring mode, individual rule matches do not block the request immediately. Instead, each triggered rule assigns an anomaly score (Critical=5, Error=4, Warning=3) based on attack severity. At the end of the request phase, the cumulative score is compared against a configured threshold (typically 5). If the threshold is exceeded, the request is blocked. This drastically reduces false positives compared to traditional immediate-blocking rules.

### Q2: What is HTTP Request Smuggling and what causes it?
**Answer**: HTTP Request Smuggling occurs when a front-end proxy and a back-end server disagree on the boundaries of an HTTP request due to differing interpretations of the `Content-Length` and `Transfer-Encoding: chunked` headers. An attacker sends an ambiguous request such that the front-end reads it as one request, but the back-end reads it as two, leaving a smuggled prefix in the TCP socket buffer that compromises subsequent requests.

### Q3: What is the purpose of the `frame-ancestors 'none'` directive in Content Security Policy?
**Answer**: It instructs compliant web browsers to refuse embedding the website inside an `<iframe>`, `<frame>`, or `<object>`, providing complete modern defense against Clickjacking attacks (replacing the legacy `X-Frame-Options: DENY` header).

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 08 - Troubleshooting](./08-Troubleshooting.md) | [Index](../../../README.md) | [10 - Hands On Practice →](./10-Hands-On-Practice.md) |
