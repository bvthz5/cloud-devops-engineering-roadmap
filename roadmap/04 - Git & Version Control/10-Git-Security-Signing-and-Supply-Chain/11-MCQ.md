# 11 - Git Security: Self-Assessment MCQs

### Q1. Which tool is officially recommended by the Git core team to purge sensitive data from repository history?
- A) `git filter-branch`
- B) `git clean -fdx`
- C) `git filter-repo`
- D) `git scrub`
<details><summary><b>View Answer</b></summary><b>Correct Answer: C</b><br>`git filter-repo` is the modern, officially recommended replacement for `git filter-branch`.</details>

---

### Q2. Since Git version 2.34, which type of cryptographic key can be used directly for commit signing without GPG?
- A) SSL Certificates
- B) SSH Keys (e.g. Ed25519 / RSA)
- C) TLS Private Keys
- D) Kerberos Tickets
<details><summary><b>View Answer</b></summary><b>Correct Answer: B</b><br>Git 2.34+ natively supports SSH key commit signing.</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [10 - Hands-On Practice](./10-Hands-On-Practice.md) | [README](./README.md) | [12 - Quick Revision](./12-Quick-Revision.md) |
