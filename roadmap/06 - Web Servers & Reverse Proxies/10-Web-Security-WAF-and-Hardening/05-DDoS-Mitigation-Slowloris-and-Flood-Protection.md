# 05 - DDoS Mitigation: Slowloris and Flood Protection

## 1. Slowloris and Slow POST Attacks

In a **Slowloris** attack, an attacker opens hundreds of TCP connections to a web server and sends partial HTTP headers extremely slowly (e.g., sending one header byte every 10 seconds).
Traditional threaded web servers keep worker threads allocated waiting for the headers to complete, exhausting thread pools with minimal attacker bandwidth!

---

## 2. Hardened Timeout Defenses in Nginx

```nginx
http {
    # Drop client if headers do not complete within 10 seconds
    client_header_timeout 10s;

    # Drop client if body bytes stall for more than 10 seconds
    client_body_timeout 10s;

    # Drop client if connection remains idle
    keepalive_timeout 30s 15s;

    # Drop socket if response bytes cannot be written
    send_timeout 10s;

    # Max body size
    client_max_body_size 10m;
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - Mitigating HTTP Request Smuggling and Splitting](./04-Mitigating-HTTP-Request-Smuggling-and-Splitting.md) | [Index](../../../README.md) | [06 - Zero Trust Edge mTLS and API Gateway Hardening →](./06-Zero-Trust-Edge-mTLS-and-API-Gateway-Hardening.md) |
