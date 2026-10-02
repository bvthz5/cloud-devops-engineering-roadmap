# 11 - Multiple-Choice Assessment (MCQ)

### 1. Which Nginx variable indicates whether a request was served from proxy cache (HIT, MISS, BYPASS)?
- [ ] A) `$cache_result`
- [x] B) `$upstream_cache_status`
- [ ] C) `$proxy_cache_state`
- [ ] D) `$http_cache_status`

<details>
<summary><b>Explanation</b></summary>
<code>$upstream_cache_status</code> contains the evaluation status of the proxy cache (HIT, MISS, BYPASS, EXPIRED, STALE, UPDATING, REVALIDATED).
</details>

---

### 2. What algorithm is implemented by Nginx's `limit_req_zone` module?
- [ ] A) Sliding Window Counter
- [x] B) Leaky Bucket
- [ ] C) Fixed Window Counter
- [ ] D) Weighted Round Robin

<details>
<summary><b>Explanation</b></summary>
Nginx's <code>limit_req</code> implements the Leaky Bucket algorithm, processing bursts up to a specified capacity and draining at a smooth, constant rate.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
