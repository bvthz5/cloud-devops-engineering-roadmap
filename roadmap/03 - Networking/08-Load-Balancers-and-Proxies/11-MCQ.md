# 11 - Load Balancers & Proxies: Self-Assessment MCQs

### Q1. What HTTP status code indicates that the load balancer reached the backend, but the backend did not respond within the configured timeout?
- A) 500 Internal Server Error
- B) 502 Bad Gateway
- C) 503 Service Unavailable
- D) 504 Gateway Timeout
<details><summary><b>View Answer</b></summary><b>Correct Answer: D</b><br>504 Gateway Timeout signifies that the upstream server timed out while processing the request.</details>

---

### Q2. Which algorithm is best suited for WebSocket connections with widely differing durations?
- A) Round Robin
- B) Least Connections
- C) IP Hash
- D) Random
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>Least Connections ensures that servers with many long-lived active WebSockets do not receive new connections until old ones terminate.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
