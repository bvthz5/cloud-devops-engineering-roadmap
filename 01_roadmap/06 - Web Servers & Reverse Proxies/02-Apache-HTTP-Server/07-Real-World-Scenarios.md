# 07 - Real-World Scenarios & Outage Post-Mortems

## Scenario 1: The Prefork Memory Collapse Outage

### Context & Incident
A company ran a high-traffic e-commerce portal on Apache. During a flash sale, the web server became completely unresponsive, SSH logins hung, and AWS CloudWatch alerted high swap usage.

### Root Cause
Apache was running the legacy `mpm_prefork` module with `MaxRequestWorkers 800`. Each Apache worker process consumed 45MB of RAM due to legacy embedded modules. Under 800 concurrent requests:
$$800 \times 45\text{MB} = 36\text{GB RAM}$$
The VM only had 16GB of RAM. The server entered aggressive kernel memory paging (thrashing) and crashed.

### Solution: Migration to `mpm_event` and PHP-FPM
1. Decouple PHP execution from Apache processes by migrating to `php-fpm`.
2. Disable `mpm_prefork` and enable `mpm_event`:
   ```bash
   sudo a2dismod php8.1 mpm_prefork
   sudo a2enmod mpm_event proxy_fcgi setenvif
   sudo a2enconf php8.1-fpm
   sudo systemctl restart apache2
   ```
Memory footprint per connection dropped from 45MB to ~2MB!

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 06 - Security Hardening and Module Management](./06-Security-Hardening-and-Module-Management.md) | [Index](../../../README.md) | [08 - Troubleshooting →](./08-Troubleshooting.md) |
