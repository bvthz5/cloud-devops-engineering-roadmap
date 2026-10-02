# 11 - DevOps and CI/CD Scenarios

In modern DevOps, `curl` and `wget` are heavily utilized inside Bash scripts, Dockerfiles, and CI/CD pipelines (like GitHub Actions or GitLab CI). 

---

## 🐳 Scenario 1: Installing Software in a Dockerfile

When building Docker images, you must keep the image size small. You download a binary, extract it, and immediately delete the downloaded archive, all in a single `RUN` instruction.

### The `curl` approach (Piping directly to tar)
This is the most elegant method. You don't even save the file to disk; you stream it directly into the extraction tool.
```dockerfile
RUN curl -sSL https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 \
    -o /usr/local/bin/jq && \
    chmod +x /usr/local/bin/jq
```

### The `wget` approach
```dockerfile
RUN wget -qO /usr/local/bin/jq https://github.com/stedolan/jq/releases/download/jq-1.6/jq-linux64 && \
    chmod +x /usr/local/bin/jq
```
*(Notice `-qO`. In `wget`, you can combine the quiet flag and the output flag).*

---

## 🪝 Scenario 2: Triggering Webhooks

CI/CD pipelines often need to notify external systems (like Slack, Microsoft Teams, or a deployment tracker) that a job has finished. This is done via Webhooks (HTTP POST requests with JSON payloads).

`curl` is the perfect tool for this.

```bash
#!/bin/bash
# Send a success message to a Slack channel

SLACK_WEBHOOK_URL="https://hooks.slack.com/services/T0000/B000/XXXX"

curl -s -X POST -H 'Content-type: application/json' \
  --data '{"text":"✅ Deployment to Production Successful!"}' \
  $SLACK_WEBHOOK_URL
```

---

## 🩺 Scenario 3: Checking Endpoint Health (Readiness Probes)

In Kubernetes or simple bash scripts, you often need to loop and wait until a web server is actually up and responding with an HTTP `200 OK` status code before proceeding.

You don't want the HTML body; you only want the HTTP status code.

```bash
#!/bin/bash
# Wait for API to become ready

echo "Waiting for API to start..."

while true; do
  # -s = silent
  # -o /dev/null = throw away the body
  # -w "%{http_code}" = print only the status code
  STATUS=$(curl -s -o /dev/null -w "%{http_code}" http://localhost:8080/health)
  
  if [ "$STATUS" -eq 200 ]; then
    echo "API is UP!"
    break
  else
    echo "API returned $STATUS, waiting 5 seconds..."
    sleep 5
  fi
done
```

---

## ⬇️ Scenario 4: Fetching Metadata inside AWS/GCP

If you are inside an AWS EC2 instance or a GCP Compute instance, you can use `curl` to query the internal metadata server (which always lives at `169.254.169.254`) to find out the instance's own IP address or IAM role.

**AWS (IMDSv2 requires a token):**
```bash
TOKEN=$(curl -X PUT "http://169.254.169.254/latest/api/token" -H "X-aws-ec2-metadata-token-ttl-seconds: 21600")
curl -H "X-aws-ec2-metadata-token: $TOKEN" -v http://169.254.169.254/latest/meta-data/local-ipv4
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Common Flags Cheat Sheet](./10-Common-Flags-Cheat-Sheet.md) | [README](./README.md) | [12 - Troubleshooting](./12-Troubleshooting.md) |
