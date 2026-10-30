# 08 — Binary & Runtime Troubleshooting Guide

---

## 1. The Binary Inspection Toolkit

```bash
# 1. Check binary architecture and linking type (Static vs Dynamic)
file /usr/bin/curl

# 2. List all dynamic shared library dependencies
ldd /usr/bin/curl

# 3. Read ELF header (entry point, interpreter path, architecture)
readelf -l /usr/bin/curl | grep interpreter

# 4. List all symbols exported or imported by a binary
nm -D /usr/bin/curl | grep curl_easy_init

# 5. Trace dynamic linker failures at runtime
LD_DEBUG=libs,symbols /usr/bin/curl https://example.com
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 07 - Real World Scenarios](./07-Real-World-Scenarios.md) | [Index](../../../README.md) | [09 - Interview QA →](./09-Interview-QA.md) |
