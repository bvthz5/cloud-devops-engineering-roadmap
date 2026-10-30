# 10 — Data Formats Troubleshooting Guide

A practical troubleshooting guide for diagnosing and fixing syntax errors, indentation traps, character encoding bugs, and schema validation failures across YAML, JSON, XML, and TOML.

---

## 1. Troubleshooting YAML Configuration Errors

### Error 1: `found character '\t' that cannot start any token`
- **Cause:** A tab character (`\t`) was used for indentation. YAML strictly forbids tabs.
- **Diagnosis:**
  ```bash
  # Display non-printable characters (tabs show as ^I)
  cat -v -t config.yaml | grep "\^I"
  ```
- **Remediation:** Replace all tabs with two spaces:
  ```bash
  sed -i 's/\t/  /g' config.yaml
  ```

---

### Error 2: `mapping values are not allowed here`
- **Cause:** Usually caused by:
  1. Missing space after a colon (e.g., `port:8080` instead of `port: 8080`).
  2. Mismatched indentation between sibling keys.
  3. A string value containing an unquoted colon (e.g., `connection: host:port`).
- **Remediation:**
  Quote strings containing colons:
  ```yaml
  connection: "host:port"
  ```

---

### Error 3: Silent Boolean Conversion ("The Norway Problem")
- **Cause:** Using unquoted `NO`, `yes`, `true`, `false`, `on`, `off` as strings.
- **Verification with `yq`:**
  ```bash
  yq eval '.shipping.country' config.yaml
  # If output is 'false' instead of 'NO' -> Quoting required!
  ```
- **Remediation:** Always wrap country codes and string values in single or double quotes.

---

## 2. Troubleshooting JSON Configuration Errors

### Error 1: `Expecting property name enclosed in double quotes`
- **Cause:** Using single quotes (`'key'`) or unquoted keys (`key:`).
- **Validation with `jq`:**
  ```bash
  jq empty config.json
  # Example output:
  # parse error: Expected string key before ':' at line 3, column 10
  ```
- **Remediation:** Ensure all object keys are wrapped strictly in double quotes (`"key": "value"`).

---

### Error 2: `Trailing comma not allowed`
- **Cause:** Leaving a comma after the final item in an object or array:
  ```json
  {
    "service": "api",
    "replicas": 3,   <-- Trailing comma!
  }
  ```
- **Remediation:** Remove the comma from the last line before the closing brace or bracket.

---

## 3. Detecting UTF-8 BOM (Byte Order Mark) Corruption

### Symptoms
A file looks visually perfect in your editor, but parsers (`jq`, `yq`, Python, or Kubernetes) fail on line 1 column 1 with:
`JSONDecodeError: Unexpected UTF-8 BOM` or `syntax error: unexpected token \ufeff`.

### Root Cause
Some Windows text editors (such as Notepad) prepend an invisible 3-byte marker (`EF BB BF`) to the beginning of UTF-8 files known as the **Byte Order Mark (BOM)**. Linux and standard POSIX parsers do not expect a BOM and choke on it.

### Diagnostic Command
```bash
# Inspect the first 5 bytes in hexadecimal
head -c 5 config.yaml | xxd -p
# If output starts with: efbbbf -> File is corrupted by UTF-8 BOM!
```

### Remediation
Strip the UTF-8 BOM cleanly:
```bash
sed -i '1s/^\xEF\xBB\xBF//' config.yaml
```

---

## 4. Validating Formats from the Command Line

```bash
# 1. Validate YAML syntax
yamllint config.yaml
# Or using python:
python3 -c "import yaml; yaml.safe_load(open('config.yaml'))" && echo "Valid YAML"

# 2. Validate JSON syntax
jq empty config.json && echo "Valid JSON"

# 3. Validate XML syntax
xmllint --noout document.xml && echo "Valid XML"

# 4. Validate TOML syntax (Python 3.11+)
python3 -c "import tomllib; tomllib.load(open('config.toml', 'rb'))" && echo "Valid TOML"
```

---

## Diagnostic Summary Cheat Sheet

| Symptom | Tool | Quick Fix |
| :--- | :--- | :--- |
| **`found character '\t'`** | `cat -t` | Replace tabs with spaces: `sed -i 's/\t/  /g'` |
| **Line 1 Column 1 Syntax Error**| `xxd -p` | Remove UTF-8 BOM: `sed -i '1s/^\xEF\xBB\xBF//'` |
| **Trailing comma in JSON** | `jq empty` | Delete comma from final element |
| **Unquoted string evaluates as boolean**| `yq` | Wrap string in single quotes (`'NO'`) |
| **XML tag mismatch** | `xmllint` | Ensure closing tags match case exactly |

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Real World Scenarios](./09-Real-World-Scenarios.md) | [Index](../../../README.md) | [11 - Interview QA →](./11-Interview-QA.md) |
