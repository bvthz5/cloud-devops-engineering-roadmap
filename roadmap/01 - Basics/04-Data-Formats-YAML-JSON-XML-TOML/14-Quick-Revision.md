# 14 — Quick Revision & Reference Cheat Sheet

A condensed, high-yield reference guide summarizing data serialization formats (YAML, JSON, XML, TOML), syntax quirks, templating, and secret security rules.

---

## 1. Format Characteristics Comparison

| Feature | JSON | YAML | XML | TOML |
| :--- | :--- | :--- | :--- | :--- |
| **Comments?** | ❌ No | ✅ Yes (`#`) | ✅ Yes (`<!-- -->`) | ✅ Yes (`#`) |
| **Whitespace?** | Unimportant | **Spaces Only (No tabs!)** | Unimportant | Unimportant |
| **Braces/Tags** | Braces `{}` | Indentation | Tags `<tag>...</tag>`| Headers `[table]` |
| **Parsing Speed**| **Ultra-fast** | Slow | Moderate | Fast |
| **Primary Use** | APIs, Terraform state | Kubernetes, CI/CD, Ansible| Maven, SAML SSO | containerd, Cargo, pyproject |

---

## 2. YAML Syntax Quick Rules

- **Tabs are strictly prohibited.** Always use 2 spaces per indentation level.
- **Colons must have a space after them:** `key: value`.
- **The Norway Problem:** Country code `NO` parses as boolean `false` in YAML 1.1 unless quoted: `'NO'`.
- **Block Scalars:**
  - `|`: Literal block (preserves all line breaks).
  - `>`: Folded block (replaces internal line breaks with spaces).
- **Anchors & Merge Keys:**
  - `&base`: Marks an anchor.
  - `*base`: References an alias.
  - `<<: *base`: Merges anchor mappings into current dictionary.

---

## 3. The 4 Formats Rosetta Stone

```json
/* JSON */
{
  "app": "orders",
  "port": 8080,
  "db": { "host": "localhost" }
}
```

```yaml
# YAML
app: orders
port: 8080
db:
  host: localhost
```

```xml
<!-- XML -->
<root>
  <app>orders</app>
  <port>8080</port>
  <db>
    <host>localhost</host>
  </db>
</root>
```

```toml
# TOML
app = "orders"
port = 8080

[db]
host = "localhost"
```

---

## 4. Configuration Security Rules

1. **Base64 is NOT Encryption:** Kubernetes `Secret` data is plaintext; never commit it to Git unencrypted.
2. **Scan for Secrets:** Run `gitleaks detect` before pushing commits to Git.
3. **Value-Level Encryption (SOPS):**
   - Encrypt values: `sops --encrypt --kms <ARN> secret.yaml > secret.enc.yaml`
   - Decrypt to stdout: `sops --decrypt secret.enc.yaml | kubectl apply -f -`
4. **Twelve-Factor App Factor III:** Store non-sensitive configuration in environment variables; pull secrets dynamically from Vault or AWS Secrets Manager.

---

## 5. Command-Line Validation Cheat Sheet

| Task | Command |
| :--- | :--- |
| **Validate JSON** | `jq empty file.json` |
| **Validate YAML** | `yamllint file.yaml` |
| **Validate XML** | `xmllint --noout file.xml` |
| **Validate TOML** | `python3 -c "import tomllib; tomllib.load(open('file.toml', 'rb'))"` |
| **Convert JSON to YAML** | `yq -p=json -o=yaml file.json` |
| **Convert YAML to JSON** | `yq -p=yaml -o=json file.yaml` |
| **In-place YAML Tag Update**| `yq -i '.spec.image = "app:v2"' deploy.yaml` |
| **Scan Repo for Secrets** | `gitleaks detect --verbose` |
| **Detect UTF-8 BOM** | `head -c 5 file.yaml \| xxd -p` (look for `efbbbf`) |
| **Remove UTF-8 BOM** | `sed -i '1s/^\xEF\xBB\xBF//' file.yaml` |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [13 - MCQ](./13-MCQ.md) | [README](./README.md) | - |
