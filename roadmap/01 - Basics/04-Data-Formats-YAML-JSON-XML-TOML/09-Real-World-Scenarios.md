# 09 — Real-World DevOps & Configuration Outage Scenarios

Format quirks and configuration mistakes cause some of the most baffling and catastrophic outages in cloud computing. Below are four real-world production post-mortems.

---

## Scenario 1: The "Norway Problem" Halting Global E-Commerce Routing

### Problem Statement
A global logistics platform deploys an international shipping microservice to Kubernetes using Helm. The configuration specifies a whitelist of supported ISO-3166-1 alpha-2 country codes:
```yaml
shipping:
  supported_countries:
    - US
    - CA
    - GB
    - NO
    - SE
```
Immediately upon deployment, customers in Norway report that orders cannot be placed. The internal backend logs report:
`ValidationError: Field 'supported_countries[3]' must be a string, got boolean (false)`.

### Architecture Analysis
The Helm chart and underlying Go/Python parser were running against **YAML 1.1 specification rules**.
- In YAML 1.1, the token `NO` is defined as a case-insensitive alternative for the boolean value **`false`** (along with `no`, `n`, `N`, `false`, `off`).
- The parser converted the country code `NO` to the boolean literal `false`.
- The strongly-typed application schema expected a string (`[A-Z]{2}`), rejected the boolean, and failed order fulfillment.

```text
YAML 1.1 Parser:
- US ──► String "US"
- CA ──► String "CA"
- NO ──► Evaluated as Boolean: FALSE ──► Application Schema Exception!
```

### Engineering Solution
1. **Explicit String Quoting:**
   ```yaml
   shipping:
     supported_countries:
       - 'US'
       - 'CA'
       - 'GB'
       - 'NO'  # Quoted strictly as a string
       - 'SE'
   ```
2. **Upgrade Parser to YAML 1.2:** YAML 1.2 removed `NO`, `yes`, `on`, and `off` from the boolean dictionary, restricting booleans strictly to `true` and `false`.
3. **Automated Linter:** Implement `yamllint` with rule `truthy: {check-keys: true}` in pre-commit hooks.

---

## Scenario 2: JSON Integer Precision Truncation in Financial API

### Problem Statement
A fintech company transitions its payments ledger from an internal database to a distributed microservice. High-value international wire transfers begin experiencing silent balance corruption. A transaction with transaction ID `9007199254740999` is processed as `9007199254740992`!

### Architecture Analysis
- The transaction ID was generated as a 64-bit integer (`int64`) in a Go backend service.
- The service serialized the event to JSON:
  ```json
  {"transaction_id": 9007199254740999, "amount": 250000.00}
  ```
- The receiving frontend and Node.js proxy parsed the payload using standard `JSON.parse()`.
- JavaScript numbers are implemented strictly as **IEEE 754 Double-Precision Floating Point numbers**.
- The maximum safe integer in IEEE 754 is $2^{53} - 1$ (`9,007,199,254,740,991` / `Number.MAX_SAFE_INTEGER`).
- Any integer beyond this threshold loses precision in the lower-order bits and is rounded to the nearest representable floating-point number.

### Engineering Solution
1. **Represent 64-Bit Integers and IDs as Strings in JSON APIs:**
   ```json
   {"transaction_id": "9007199254740999", "amount": "250000.00"}
   ```
2. **Best Practice:** Never transmit raw 64-bit integers (`int64`, `uint64`) or arbitrary-precision currency numbers as raw JSON numbers. Always encode them as strings.

---

## Scenario 3: XML "Billion Laughs" Denial of Service Attack

### Problem Statement
A legacy enterprise banking portal exposes a SOAP XML endpoint. An external penetration test sends an innocent-looking 1 KB XML payload. Within 3 seconds of receiving the payload, the web server's physical memory skyrockets to 100%, CPU hits 100%, and the server kernel panics with an Out-Of-Memory (OOM) crash.

### Architecture Analysis
The application's XML parser had **External Entity Expansion (XXE)** enabled. The attacker submitted an XML document with recursively nested entity references:

```xml
<?xml version="1.0"?>
<!DOCTYPE lolz [
 <!ENTITY lol "lol">
 <!ENTITY lol1 "&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;&lol;">
 <!ENTITY lol2 "&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;&lol1;">
 <!ENTITY lol3 "&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;&lol2;">
 <!ENTITY lol4 "&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;&lol3;">
 <!ENTITY lol5 "&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;&lol4;">
]>
<data>&lol5;</data>
```

When the parser resolved `&lol5;`, it expanded exponentially ($10^5 = 100,000$ strings, and higher levels scale into billions of gigabytes of RAM). This classic attack is known as the **Billion Laughs Attack** or XML Entity Expansion Bomb.

### Engineering Solution
Configure the XML parser to **disable DTD processing and external entity resolution completely**:
```python
# In Python:
import defusedxml.minidom  # Use defusedxml package which blocks entity expansion
```
In Java:
```java
DocumentBuilderFactory dbf = DocumentBuilderFactory.newInstance();
dbf.setFeature("http://apache.org/xml/features/disallow-doctype-decl", true);
```

---

## Scenario 4: Leaked AWS Access Key via Unencrypted Git Commit

### Problem Statement
A junior engineer creates a Kubernetes secret manifest:
```yaml
apiVersion: v1
kind: Secret
metadata:
  name: aws-creds
data:
  aws_access_key: QUtJQTAxMjM0NTY3ODk= # Base64 encoded AKIA...
```
The engineer commits the file to a public GitHub repository. Within 4 minutes, automated threat-actor bot crawlers scrape the key from the Git commit, generate 50 GPU instances (`g4dn.12xlarge`) in AWS `us-west-2`, and initiate cryptocurrency mining. The company receives a $28,000 AWS bill the next morning.

### Architecture Analysis
- Base64 encoding is an encoding mechanism, **NOT encryption**.
- Git repositories are immutable; even if the engineer runs `git rm` and commits again, the secret remains permanently visible in the **Git commit history**.

### Engineering Solution
1. **Immediate Revocation:** In AWS IAM, deactivate the compromised access key immediately.
2. **Pre-Commit Hook Enforcement:** Install **GitLeaks** as a pre-commit hook across the engineering organization:
   ```bash
   gitleaks protect --staged
   ```
3. **Adopt Mozilla SOPS or External Secrets Operator:** Raw credentials must never touch Git in plaintext or base64.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [08 - Configuration Security Secrets and SOPS](./08-Configuration-Security-Secrets-and-SOPS.md) | [README](./README.md) | [10 - Troubleshooting](./10-Troubleshooting.md) |
