# Data Formats — YAML, JSON, XML, and TOML

Welcome to the definitive guide on **Data Formats, Serialization, and Configuration Engineering** for Cloud, DevOps, and Platform Engineers. In modern cloud architecture, code is declarative: Kubernetes manifests, Terraform state, Helm charts, CI/CD pipelines, container specifications, and cloud API payloads are defined using structured data formats.

---

## Complete Topic Syllabus & Roadmap Mapping

Below is the complete syllabus mapping all **90 fundamental topics** plus **modern secret management, schema validation, and serialization protocols**:

### 1. Data Foundations & Serialization (Topics 1–6)
1. Structured Data
2. Data Serialization (In-memory object ──► Byte/Text stream)
3. Data Deserialization (Stream ──► In-memory data structure)
4. Data Representation (Hierarchical vs Flat vs Tabular)
5. Configuration Files
6. Configuration Management (Declarative vs Imperative)

### 2. JSON — JavaScript Object Notation (Topics 7–21)
7. JSON (RFC 8259, ECMA-404)
8. JSON Syntax
9. JSON Objects (`{}`)
10. JSON Key-Value Pairs
11. JSON Arrays (`[]`)
12. JSON Strings (UTF-8, escape sequences `\"`)
13. JSON Numbers (Integer, float, scientific notation `1e6`)
14. JSON Booleans (`true`, `false`)
15. JSON Null (`null`)
16. JSON Nesting (Deep structures)
17. JSON Validation
18. JSON Schema (Draft 7 / 2020-12, type checking, validation rules)
19. JSON Parsing
20. JSON Serialization (`json.dumps()`, `JSON.stringify()`)
21. JSON Deserialization (`json.loads()`, `JSON.parse()`)

### 3. YAML — YAML Ain't Markup Language (Topics 22–45)
22. YAML (Human-friendly data serialization)
23. YAML Syntax
24. YAML Documents (Multi-document streams separated by `---` and `...`)
25. YAML Mappings (Hash / Dictionary key: value)
26. YAML Sequences (Lists with hyphens `- item`)
27. YAML Scalars (Atomic values)
28. YAML Strings (Unquoted, single-quoted, double-quoted)
29. YAML Numbers (Integers, floats, hex `0x`, octal `0o`)
30. YAML Booleans & **The Norway Problem** (`y`, `yes`, `true`, `on` vs `n`, `no`, `false`, `off`)
31. YAML Null (`null`, `~`, empty values)
32. YAML Indentation (Spaces strictly enforced, tabs strictly prohibited!)
33. YAML Nesting
34. YAML Comments (`#`)
35. YAML Multiline Strings
36. YAML Literal Blocks (`|` preserves newlines)
37. YAML Folded Blocks (`>` replaces newlines with spaces)
38. YAML Anchors (`&anchor_name`)
39. YAML Aliases (`*anchor_name`)
40. YAML Merge Keys (`<<: *anchor_name`)
41. YAML Type Inference
42. YAML Validation (`yamllint`, `kubeval`, `kubeconform`)
43. YAML Parsing
44. YAML Serialization
45. YAML Deserialization

### 4. XML — Extensible Markup Language (Topics 46–61)
46. XML (W3C standard)
47. XML Syntax
48. XML Declaration (`<?xml version="1.0" encoding="UTF-8"?>`)
49. XML Elements (Start and end tags `<tag>...</tag>`)
50. XML Attributes (`<tag id="123">`)
51. XML Text (PCDATA vs CDATA `<![CDATA[...]]>`)
52. XML Nesting & Root element constraint
53. XML Namespaces (`xmlns:prefix="uri"`)
54. XML Entities (`&lt;`, `&gt;`, `&amp;`, `&quot;`, `&apos;`)
55. XML Comments (`<!-- ... -->`)
56. XML Schema (XSD)
57. XSD Validation
58. XML Validation (`xmllint`)
59. XML Parsing (DOM tree vs SAX streaming parser)
60. XML Serialization
61. XML Deserialization

### 5. TOML — Tom's Obvious, Minimal Language (Topics 62–74)
62. TOML (Designed by Tom Preston-Werner)
63. TOML Syntax
64. TOML Key-Value Pairs
65. TOML Strings (Basic `"..."`, Literal `'...'`, Multiline `"""..."""`)
66. TOML Numbers (Integers, floats, digit underscores `1_000_000`, hex, octal)
67. TOML Booleans (`true`, `false` lowercase)
68. TOML Arrays (Homogeneous and mixed arrays)
69. TOML Tables (`[server]`)
70. TOML Nested Tables (`[server.database]`)
71. TOML Arrays of Tables (`[[products]]`)
72. TOML Dates and Times (RFC 3339 timestamps `2026-10-02T07:30:00Z`)
73. TOML Parsing
74. TOML Validation

### 6. Format Comparisons & Trade-offs (Topics 75–78)
75. YAML vs JSON (Superset relationship, speed vs human readability)
76. YAML vs TOML (Nesting vs flat tables, Rust Cargo/Python pyproject)
77. JSON vs XML (Payload size, schema complexity, SOAP vs REST)
78. XML vs JSON

### 7. Configuration Lifecycle & Production Practices (Topics 79–90)
79. Configuration vs Secrets (Plaintext config vs sensitive credentials)
80. Environment-Specific Configuration
81. Development Configuration
82. Testing Configuration
83. Staging Configuration
84. Production Configuration
85. Configuration Validation (Schema enforcement, linters, pre-commit hooks)
86. Configuration Security (GitLeaks, Mozilla SOPS, Sealed Secrets, HashiCorp Vault)
87. Configuration Versioning (GitOps, Git branching, semver)
88. Configuration Templates (Helm, Jinja2, `envsubst`)
89. Configuration Interpolation (Variable replacement, environment substitution)
90. Configuration Best Practices (Twelve-Factor App Factor III, single source of truth, immutability)

---

## File Structure in this Module

- [`01-Structured-Data-Serialization-and-Representations.md`](./01-Structured-Data-Serialization-and-Representations.md)
- [`02-JSON-Deep-Dive-Syntax-Schema-and-Parsing.md`](./02-JSON-Deep-Dive-Syntax-Schema-and-Parsing.md)
- [`03-YAML-Deep-Dive-Syntax-Anchors-and-Gotchas.md`](./03-YAML-Deep-Dive-Syntax-Anchors-and-Gotchas.md)
- [`04-XML-Deep-Dive-Syntax-Namespaces-and-Schemas.md`](./04-XML-Deep-Dive-Syntax-Namespaces-and-Schemas.md)
- [`05-TOML-Deep-Dive-Syntax-Tables-and-Typing.md`](./05-TOML-Deep-Dive-Syntax-Tables-and-Typing.md)
- [`06-Comparison-Matrices-JSON-YAML-XML-TOML.md`](./06-Comparison-Matrices-JSON-YAML-XML-TOML.md)
- [`07-Config-Management-Environments-and-Templating.md`](./07-Config-Management-Environments-and-Templating.md)
- [`08-Configuration-Security-Secrets-and-SOPS.md`](./08-Configuration-Security-Secrets-and-SOPS.md)
- [`09-Real-World-Scenarios.md`](./09-Real-World-Scenarios.md)
- [`10-Troubleshooting.md`](./10-Troubleshooting.md)
- [`11-Interview-QA.md`](./11-Interview-QA.md)
- [`12-Hands-On-Practice.md`](./12-Hands-On-Practice.md)
- [`13-MCQ.md`](./13-MCQ.md)
- [`14-Quick-Revision.md`](./14-Quick-Revision.md)

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| - | You are here | [01 - Structured Data Serialization and Representations](./01-Structured-Data-Serialization-and-Representations.md) |
