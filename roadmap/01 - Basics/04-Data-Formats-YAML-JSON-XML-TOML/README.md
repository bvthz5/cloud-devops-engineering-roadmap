# 04-Data Formats — YAML, JSON, XML, TOML

> Structured data formats, serialization, deserialization, YAML indentation rules, JSON schemas, XML enterprise attributes, TOML configurations, format comparison matrices, and secret management.

---

## 🎯 Learning Objectives

By the end of this module, you will understand:
1. **Structured Data Foundations:** Why configuration format choices matter across Kubernetes, Ansible, Docker Compose, Terraform, GitHub Actions, and REST APIs.
2. **JSON (JavaScript Object Notation):** Syntax rules, supported data types, strict formatting (double quotes, no trailing commas, no standard comments), and API payloads.
3. **YAML (YAML Ain't Markup Language):** Indentation rules, key-value mappings, lists, multiline scalar blocks (`|` vs `>`), boolean edge cases, and anchors (`&` / `*`).
4. **XML (Extensible Markup Language):** Elements, attributes, namespaces (`xmlns`), schema validation (XSD), and legacy enterprise integration.
5. **TOML (Tom's Obvious Minimal Language):** Tables `[table]`, inline arrays, arrays of tables `[[table]]`, and application config formats (Cargo, Pyproject, Hugo).
6. **Configuration vs Secrets Management:** Environment-specific configs, environment variables, secret stores (Vault, K8s Secrets, AWS Secrets Manager).

---

## 📁 Module Navigation

- [`01-Basics.md`](01-Basics.md) — Comprehensive guide comparing YAML, JSON, XML, TOML, rules, and syntax.
- [`04-Practical-Examples.md`](04-Practical-Examples.md) — Real-world config examples, conversions, syntax validation, and linters (`yq`, `jq`).
