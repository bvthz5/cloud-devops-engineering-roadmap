# 15. Logging Formats & Log Analysis: Access & Error Logs

## Standard Log Locations & Formats

- **Nginx Access Log:** `/var/log/nginx/access.log`
- **Nginx Error Log:** `/var/log/nginx/error.log`
- **Apache Access Log:** `/var/log/apache2/access.log`
- **Apache Error Log:** `/var/log/apache2/error.log`

## Nginx Combined Log Format
```nginx
log_format combined '$remote_addr - $remote_user [$time_local] '
                    '"$request" $status $body_bytes_sent '
                    '"$http_referer" "$http_user_agent"';
```

## Structured JSON Logging (DevOps Best Practice)
Structured logs allow direct parsing in ELK Stack, Datadog, Grafana Loki, or Splunk:

```nginx
log_format json_analytics escape=json
  '{'
    '"time_local":"$time_local",'
    '"remote_addr":"$remote_addr",'
    '"request":"$request",'
    '"status": "$status",'
    '"body_bytes_sent":"$body_bytes_sent",'
    '"request_time":"$request_time",'
    '"upstream_response_time":"$upstream_response_time",'
    '"http_referrer":"$http_referer",'
    '"http_user_agent":"$http_user_agent"'
  '}';

access_log /var/log/nginx/access_json.log json_analytics;
```

## CLI Log Analysis Commands

```bash
# Count total HTTP 502 Bad Gateway errors
grep " 502 " /var/log/nginx/access.log | wc -l

# Extract top 10 requesting IP addresses
awk '{print $1}' /var/log/nginx/access.log | sort | uniq -c | sort -nr | head -n 10

# Real-time web log viewer
sudo goaccess /var/log/nginx/access.log -o /var/www/html/report.html --log-format=COMBINED
```
