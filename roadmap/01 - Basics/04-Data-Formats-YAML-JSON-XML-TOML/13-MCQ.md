# 13 — Multiple Choice Questions (Self-Assessment)

Test your mastery of Data Formats, Serialization, Declarative Configurations, and Secret Management. Each question contains four options and an expandable answer with a detailed technical explanation.

---

### Q1: What is the primary difference between Data Serialization and Data Deserialization?
- **A)** Serialization encrypts data; deserialization decrypts data.
- **B)** Serialization converts in-memory data structures into a standardized byte or text stream; deserialization reconstructs the original in-memory data objects from that stream.
- **C)** Serialization applies only to relational databases; deserialization applies only to NoSQL databases.
- **D)** Serialization formats text as XML; deserialization converts XML to JSON.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Serialization (marshalling/encoding) transforms complex in-memory programming objects into sequential byte or character streams suitable for storage or network transmission. Deserialization (unmarshalling/decoding) parses the stream to rebuild the original data structures in RAM.
</details>

---

### Q2: Which of the following statements regarding RFC 8259 JSON syntax is TRUE?
- **A)** Keys in JSON objects can be written without quotes if they contain no special characters.
- **B)** Single quotes (`'...'`) can be used interchangeably with double quotes (`"..."`) for string values.
- **C)** Trailing commas are permitted after the final element in an array.
- **D)** Comments are strictly forbidden in standard JSON documents.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: D**  
**Explanation:** Standard JSON strictly prohibits comments. All keys must be strings enclosed in double quotes (`"key"`), strings must use double quotes, and trailing commas are considered syntax errors.
</details>

---

### Q3: What happens when an unquoted value `NO` is parsed by a YAML 1.1 compliant parser?
- **A)** It is parsed as the two-character string `"NO"`.
- **B)** It raises an immediate syntax error.
- **C)** It is automatically evaluated as the Boolean value `false`.
- **D)** It is treated as a null value.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** In YAML 1.1, the tokens `y`, `yes`, `true`, `on` and `n`, `no`, `NO`, `false`, `off` are parsed as booleans (`true` or `false`). This is known as "The Norway Problem", causing ISO country code `NO` to evaluate as boolean `false` unless explicitly quoted.
</details>

---

### Q4: Which indentation character is strictly illegal in YAML?
- **A)** A single space
- **B)** Two spaces
- **C)** Four spaces
- **D)** A Tab character (`\t`)

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: D**  
**Explanation:** YAML strictly prohibits the use of tab characters for indentation. All indentation must be constructed exclusively from space characters.
</details>

---

### Q5: What is the functional difference between the YAML Literal Block scalar (`|`) and Folded Block scalar (`>`)?
- **A)** `|` converts text to uppercase; `>` converts text to lowercase.
- **B)** `|` preserves literal newlines and line breaks; `>` replaces internal newlines with spaces, folding text into a single paragraph.
- **C)** `|` encrypts the text block; `>` exposes it in plaintext.
- **D)** `|` is used for numbers; `>` is used for strings.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The pipe (`|`) block scalar operator preserves every newline exactly as written (ideal for shell scripts in ConfigMaps). The greater-than (`>`) operator folds consecutive lines into a single continuous string separated by spaces (ideal for long descriptions).
</details>

---

### Q6: In YAML, what is the role of the merge key operator (`<<:`) when paired with an alias?
- **A)** It appends a list to an existing list.
- **B)** It merges all key-value mappings from an anchored dictionary into the current map, allowing individual keys to be overridden.
- **C)** It creates a bitwise shift in numeric values.
- **D)** It deletes duplicate keys from the file.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** The merge key (`<<: *anchor_name`) injects all key-value pairs from the referenced anchor into the current mapping dictionary, providing a clean way to reuse base service templates in Docker Compose and CI/CD manifests.
</details>

---

### Q7: What is the purpose of an XML `<![CDATA[ ... ]]>` block?
- **A)** It declares the cryptographic digital signature of an XML document.
- **B)** It compresses the enclosed XML elements to reduce network bandwidth.
- **C)** It instructs the XML parser to treat the enclosed text as character data only, without interpreting characters like `<` and `&` as markup elements.
- **D)** It imports external XSD schema definitions.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** CDATA (Character Data) blocks tell the XML parser to ignore markup syntax inside the block, allowing code, shell scripts, and SQL containing `<` and `&` to be embedded without manually escaping them into entity references (`&lt;`).
</details>

---

### Q8: How does TOML declare an Array of Tables (a list of dictionaries)?
- **A)** Using square brackets: `[table]`
- **B)** Using double square brackets: `[[table]]`
- **C)** Using curly braces: `{{table}}`
- **D)** Using hyphens: `- table`

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** In TOML, a single bracket header `[server]` creates a single dictionary table. A double bracket header `[[services]]` declares that the table is an element of an array; each subsequent `[[services]]` header appends a new dictionary item to the array.
</details>

---

### Q9: Which data format provides native, first-class support for RFC 3339 Date and Time timestamps without requiring custom string parsing?
- **A)** JSON
- **B)** XML
- **C)** TOML
- **D)** CSV

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** TOML features first-class native date and time primitives (`2026-10-02T07:30:00Z`). In JSON and standard YAML, dates must be written as strings and parsed manually by application code.
</details>

---

### Q10: Why is storing Kubernetes Secret values encoded in Base64 NOT considered secure?
- **A)** Because Base64 can only be decoded by root.
- **B)** Because Base64 is an encoding format designed for safe byte transmission, not encryption; anyone with access to the manifest can decode it into plaintext in one command (`base64 -d`).
- **C)** Because Base64 corrupts binary data.
- **D)** Because Base64 strings expire after 24 hours.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: B**  
**Explanation:** Base64 is a reversible encoding scheme, not cryptographic encryption. Committing base64-encoded secrets to Git exposes credentials in plaintext to anyone who can read the repository.
</details>

---

### Q11: How does Mozilla SOPS preserve Git version control usability when encrypting configuration files?
- **A)** It stores encrypted files in a separate Git branch.
- **B)** It encrypts the entire file into a binary blob.
- **C)** It encrypts only the values, leaving the keys, hierarchy, and metadata in plaintext so meaningful Git diffs can be viewed.
- **D)** It disables Git commit verification.

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** Mozilla SOPS performs value-level encryption. By leaving keys and structural formatting unencrypted, engineers can review clear Git diffs, manage branches, and verify configuration layout without exposing sensitive credentials.
</details>

---

### Q12: Which tool is specifically designed to detect hardcoded secrets and API keys committed to Git repositories?
- **A)** `jq`
- **B)** `yamllint`
- **C)** `GitLeaks`
- **D)** `envsubst`

<details>
<summary>▶ View Answer & Explanation</summary>

**Correct Answer: C**  
**Explanation:** GitLeaks is an automated security scanner that uses regular expressions and entropy checks to identify hardcoded passwords, private keys, and cloud credentials in Git repositories and pre-commit hooks.
</details>

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [12 - Hands On Practice](./12-Hands-On-Practice.md) | [README](./README.md) | [14 - Quick Revision](./14-Quick-Revision.md) |
