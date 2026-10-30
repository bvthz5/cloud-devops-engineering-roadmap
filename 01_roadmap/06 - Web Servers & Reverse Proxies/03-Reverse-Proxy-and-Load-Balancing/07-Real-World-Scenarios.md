# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Disk Trashing Buffer Overflow Outage

### Context & Incident
A video processing platform deployed Nginx in front of a Python microservice that generates 150MB CSV analytical exports. Under high concurrency, Nginx servers froze, disk I/O utilization reached 100%, and incoming API calls hung.

### Root Cause
`proxy_buffering` was set to `on`, but `proxy_buffers` was only 64KB. Because the responses were 150MB, Nginx wrote the overflow to disk (`/var/lib/nginx/tmp/proxy/`). Under 200 concurrent downloads:
$$200 \times 150\text{MB} = 30\text{GB of rapid disk writes!}$$
The local SSD saturated with disk I/O, blocking worker processes from reading configuration or writing logs.

### Solution: Disable Buffering for Streaming Routes
```nginx
location /exports/ {
    proxy_pass http://export_backend;
    
    # Disable buffering for large streaming endpoints
    proxy_buffering off;
    proxy_request_buffering off;
}
```

---

## Scenario 2: Upstream Connection Leak & TIME_WAIT Exhaustion

### Context & Incident
A microservices mesh experienced intermittent connection resets. The proxy host logs reported:
`bind() to 0.0.0.0:xxxx failed (99: Cannot assign requested address)`

### Root Cause
Nginx was forwarding 10,000 requests per second to backend services without `keepalive` enabled in the `upstream` block. Each request opened a local ephemeral port, closed it, and placed the socket into `TIME_WAIT` for 60 seconds, exhausting all 65,535 local ports.

### Solution: Enable Upstream Keepalive Pools
```nginx
upstream backend {
    server 10.0.1.10:8080;
    keepalive 256;
}
```
And tune Linux kernel socket reuse:
```bash
sysctl -w net.ipv4.tcp_tw_reuse=1
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Header Manipulation and Client IP Preservation](./06-Header-Manipulation-and-Client-IP-Preservation.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
