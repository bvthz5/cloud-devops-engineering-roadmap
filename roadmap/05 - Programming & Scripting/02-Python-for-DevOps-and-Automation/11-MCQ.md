# 11 - Python Automation: Self-Assessment MCQs

### Q1. Why is `yaml.safe_load()` mandatory instead of `yaml.load()`?
- A) `yaml.load()` is deprecated in Python 3.10
- B) `yaml.load()` can construct arbitrary Python objects and execute malicious code embedded in YAML files
- C) `yaml.safe_load()` converts YAML directly to C code
- D) `yaml.load()` cannot parse strings
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`yaml.load()` can deserialize arbitrary Python objects, allowing remote code execution.</details>

---

### Q2. Which `subprocess.run` argument ensures an exception is raised automatically if the external process exits with a non-zero code?
- A) `raise_on_error=True`
- B) `check=True`
- C) `strict=True`
- D) `capture_output=True`
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`check=True` raises `CalledProcessError` on failure.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
