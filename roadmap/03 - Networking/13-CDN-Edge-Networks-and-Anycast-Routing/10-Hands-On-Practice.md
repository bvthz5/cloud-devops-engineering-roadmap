# 10 - Hands-On Practice: Testing CDN Cache Behaviors

## Lab Scenario
Configure an NGINX origin to serve static and dynamic content with fine-grained cache directives and verify behavior using `curl`.

---

## Lab Steps

### Step 1: Configure NGINX Cache Directives
```nginx
# /etc/nginx/conf.d/cache_demo.conf
server {
    listen 8080;

    # Static assets: Cache for 1 year, immutable
    location /static/ {
        add_header Cache-Control "public, max-age=31536000, immutable";
        return 200 "Static Asset Payload";
    }

    # API endpoints: Stale while revalidate
    location /api/data {
        add_header Cache-Control "public, max-age=10, s-maxage=30, stale-while-revalidate=60";
        return 200 "Dynamic API Data";
    }
}
```

### Step 2: Validate Headers Using curl
```bash
curl -I http://localhost:8080/static/logo.png
curl -I http://localhost:8080/api/data
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [09 - Interview Q&A](./09-Interview-QA.md) | [README](./README.md) | [11 - Self-Assessment MCQ](./11-MCQ.md) |
