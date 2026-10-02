# 05 - Stream Processing with jq, awk, and sed

## 1. Parsing JSON with `jq`

```bash
# Extract value from JSON object
INSTANCE_ID=$(curl -s http://169.254.169.254/latest/dynamic/instance-identity/document | jq -r '.instanceId')

# Filter array of objects: Select names of running pods
kubectl get pods -o json | jq -r '.items[] | select(.status.phase=="Running") | .metadata.name'

# Construct JSON payload dynamically
jq -n --arg env "prod" --arg version "v1.2.3" '{environment: $env, release: $version, active: true}'
```

---

## 2. Text Slicing with `awk`

```bash
# Print 1st and 3rd column from space-delimited output
kubectl get pods | awk '{print $1, $3}'

# Filter by column condition: Print process name if memory > 20%
ps aux | awk '$4 > 20.0 {print $11, $4"%"}'
```

---

## 3. In-Place Editing with `sed`

```bash
# Replace string in-place on Linux:
sed -i 's/PORT=3000/PORT=8080/g' .env

# Cross-platform safe in-place replacement (works on macOS and Linux):
sed -i.bak 's/DEBUG=true/DEBUG=false/g' config.ini && rm -f config.ini.bak
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Error Handling & Traps](./04-Error-Handling-Traps-and-Signal-Management.md) | [README](./README.md) | [06 - Subprocesses & Redirection](./06-Subprocesses-Redirection-and-Process-Substitution.md) |
