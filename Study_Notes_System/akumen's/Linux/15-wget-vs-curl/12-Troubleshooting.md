# 12 - Troubleshooting Checklists

When your network requests fail, don't blindly retry. Follow these steps to isolate the issue.

---

## 🟥 Scenario: Connection Refused / Connection Timeout

**The Error:** `curl: (7) Failed to connect to example.com port 443: Connection refused` or `Connection timed out`.

1.  **Check DNS:** Is the domain resolving to an IP address?
    *   *Action:* Look at the first line of verbose output (`curl -v`). Does it show an IP? If not, the DNS server is failing, or you have a typo in the URL.
2.  **Check the Port:** Are you hitting the right port? (HTTP is 80, HTTPS is 443).
    *   *Action:* Explicitly define the port if it's non-standard: `curl http://example.com:8080`.
3.  **Check Firewalls / Security Groups:**
    *   *Action:* A "Connection Refused" usually means the server is up, but no service is listening on that port. A "Timeout" usually means a firewall (like an AWS Security Group or `iptables`) is silently dropping your packets.

---

## 🟧 Scenario: SSL/TLS Certificate Errors

**The Error:** `curl: (60) SSL certificate problem: unable to get local issuer certificate` or `wget: unable to establish SSL connection`.

1.  **The Cause:** The server is presenting an SSL certificate that your machine does not trust. This happens often with self-signed certificates in internal corporate networks, or if the CA (Certificate Authority) bundle on your Linux machine is outdated.
2.  **The Fix (Insecure Bypass):** If you are in a dev environment and *know* the server is safe, you can tell the tools to ignore the warning.
    *   `curl -k https://untrusted.com` (or `--insecure`)
    *   `wget --no-check-certificate https://untrusted.com`
3.  **The Fix (Proper):** Update your system's CA certificates (`sudo apt-get install --reinstall ca-certificates`).

---

## 🟨 Scenario: 403 Forbidden / 401 Unauthorized

**The Error:** The connection succeeds, but the HTTP response code is 400-level.

1.  **Check Authentication:** Did you pass the correct Bearer token or Basic Auth credentials? Are there typos in the Header?
    *   *Action:* Re-run with `curl -v` and inspect the `> Authorization:` header being sent.
2.  **Check User-Agent:** Is a WAF (Web Application Firewall) blocking command-line tools?
    *   *Action:* Spoof a browser User-Agent (`curl -A "Mozilla/5.0..."`).
3.  **Check the Path:** Are you authorized to access that specific endpoint?

---

## 🟩 Scenario: "It works in Postman, but fails in curl!"

This is the most common frustration for junior engineers. Postman hides a lot of complexity.

1.  **Headers:** Postman automatically injects headers like `Accept: */*` and `Content-Type`. You must manually specify these in `curl` if the API requires them.
2.  **Data Formatting:** If sending JSON, ensure your single and double quotes are correct in bash.
    *   *Wrong:* `-d {"key": "value"}` (Bash strips the quotes).
    *   *Right:* `-d '{"key": "value"}'`
3.  **Use Postman's Code Generator:**
    *   If a request works in Postman, look for the `</>` (Code) button on the right side of the screen.
    *   Select `cURL` from the dropdown. Postman will generate the exact `curl` command for you, including all the hidden headers. Paste this into your terminal.
