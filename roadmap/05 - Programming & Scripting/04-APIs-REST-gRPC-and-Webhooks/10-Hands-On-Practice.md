# 10 - Hands-On Practice: Building an HMAC-Verified Webhook Receiver

## Lab Scenario
Create a lightweight Python webhook receiver that cryptographically validates incoming payloads using HMAC-SHA256 signatures before processing.

---

## Lab Steps

### Step 1: Write `webhook_server.py`
```python
cat << 'EOF' > /tmp/webhook_server.py
from http.server import HTTPServer, BaseHTTPRequestHandler
import hmac
import hashlib
import json

WEBHOOK_SECRET = "super-secret-devops-token"

class WebhookHandler(BaseHTTPRequestHandler):
    def do_POST(self):
        content_length = int(self.headers.get("Content-Length", 0))
        raw_payload = self.rfile.read(content_length)
        received_sig = self.headers.get("X-Signature-256", "")

        # Compute HMAC-SHA256
        expected_sig = "sha256=" + hmac.new(
            WEBHOOK_SECRET.encode("utf-8"),
            raw_payload,
            hashlib.sha256
        ).hexdigest()

        if not hmac.compare_digest(expected_sig, received_sig):
            self.send_response(401)
            self.end_headers()
            self.wfile.write(b"Unauthorized: Signature Mismatch")
            return

        self.send_response(200)
        self.end_headers()
        self.wfile.write(b"Webhook Verified & Accepted")
        print("Successfully processed verified event:", raw_payload.decode("utf-8"))

server = HTTPServer(("127.0.0.1", 9090), WebhookHandler)
print("Listening for webhooks on port 9090...")
server.serve_forever()
EOF
```

### Step 2: Test with Valid Signature
```bash
# In another terminal: Compute signature and send valid request
python3 -c "
import hmac, hashlib, requests
secret = 'super-secret-devops-token'
body = b'{"event": "deployment_success"}'
sig = 'sha256=' + hmac.new(secret.encode(), body, hashlib.sha256).hexdigest()
r = requests.post('http://127.0.0.1:9090', data=body, headers={'X-Signature-256': sig})
print(r.status_code, r.text)
"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
