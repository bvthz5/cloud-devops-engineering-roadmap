# 23. Scenario-Based Troubleshooting Challenges

## Challenge 1: The Infinite HTTPS Redirect Loop
- **Scenario:** A developer deploys Nginx behind an AWS Application Load Balancer (ALB). Accessing `https://example.com` results in `ERR_TOO_MANY_REDIRECTS`.
- **Root Cause Analysis:** The ALB terminates SSL and forwards HTTP (port 80) to Nginx. Nginx sees HTTP and issues a 301 redirect to HTTPS, sending the client back to ALB, which forwards HTTP again.
- **Solution:** Configure Nginx to check `X-Forwarded-Proto` before redirecting:
  ```nginx
  if ($http_x_forwarded_proto != 'https') {
      return 301 https://$host$request_uri;
  }
  ```

## Challenge 2: "Worker Connections Exceeded" During Flash Sale
- **Scenario:** Nginx error log fills with `1024 worker_connections are not enough`.
- **Root Cause Analysis:** Default `worker_connections` setting of 1024 was exhausted by high traffic spikes.
- **Solution:** Increase connection limits in `nginx.conf` and adjust Linux kernel `ulimit`:
  ```nginx
  events {
      worker_connections 10240;
  }
  ```
  Run: `ulimit -n 65535`
