# 11 - Bash Scripting: Self-Assessment MCQs

### Q1. Which flag causes a Bash script to terminate immediately if an unbound/undefined variable is evaluated?
- A) `-e`
- B) `-u`
- C) `-x`
- D) `-f`
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>`-u` (nounset) exits on any reference to an undefined variable.</details>

---

### Q2. In Bash parameter expansion, what does `${VAR:-default}` do?
- A) Throws an error if VAR is unset
- B) Assigns 'default' to VAR permanently
- C) Evaluates to 'default' if VAR is unset or empty, without changing VAR
- D) Removes 'default' from the string VAR
<details><summary><b>View Answer</b></summary><b>Correct Answer: C</b><br>`${VAR:-default}` provides a fallback value without mutating the original variable.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 10 - Hands On Practice](./10-Hands-On-Practice.md) | [Index](../../../README.md) | [12 - Quick Revision →](./12-Quick-Revision.md) |
