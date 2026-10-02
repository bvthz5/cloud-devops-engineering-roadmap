# 02 - Variables, Arrays, and Parameter Expansion

## 1. Parameter Expansion Mastery

Avoid clumsy `if [ -z "$VAR" ]` blocks. Use built-in Bash parameter expansions:

```bash
# 1. Use default value if variable is unset or empty:
PORT="${APP_PORT:-8080}"

# 2. Assign default value to variable if unset:
: "${ENVIRONMENT:=production}"

# 3. Throw fatal error if variable is unset:
AWS_REGION="${AWS_DEFAULT_REGION:?Error: AWS_DEFAULT_REGION must be defined!}"

# 4. String Slicing: ${VAR:offset:length}
FILENAME="backup-2026-10-02.tar.gz"
echo "${FILENAME:0:6}"   # Output: backup

# 5. Pattern Stripping:
# % removes shortest match from end:
echo "${FILENAME%.tar.gz}" # Output: backup-2026-10-02
# # removes shortest match from beginning:
echo "${FILENAME#backup-}" # Output: 2026-10-02.tar.gz
```

---

## 2. Indexed and Associative Arrays

```bash
# Indexed Array
SERVICES=("auth" "billing" "orders")
SERVICES+=("notifications")
echo "Total services: ${#SERVICES[@]}"
for SVC in "${SERVICES[@]}"; do
    echo "Restarting service: $SVC"
done

# Associative Array (Dictionary / Map) - Requires Bash 4+
declare -A PORT_MAP=(
    ["auth"]=8001
    ["billing"]=8002
    ["orders"]=8003
)
echo "Auth port: ${PORT_MAP["auth"]}"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Strict Mode](./01-Bash-Strict-Mode-and-Script-Anatomy.md) | [README](./README.md) | [03 - Argument Parsing](./03-CLI-Argument-Parsing-with-getopts.md) |
