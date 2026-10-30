# 10 — Hands-On Practice Labs: Binary & Encoding

---

## Lab 1: Fixing a Broken CRLF Container Entrypoint Script

```bash
# 1. Create a script with Windows CRLF line endings intentionally
printf '#!/bin/sh
echo "Hello from inside container!"
' > /tmp/bad_script.sh
chmod +x /tmp/bad_script.sh

# 2. Inspect with cat -v to see the hidden ^M
cat -v /tmp/bad_script.sh

# 3. Strip CRLF using sed
sed -i 's/$//' /tmp/bad_script.sh

# 4. Verify with file command
file /tmp/bad_script.sh
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
