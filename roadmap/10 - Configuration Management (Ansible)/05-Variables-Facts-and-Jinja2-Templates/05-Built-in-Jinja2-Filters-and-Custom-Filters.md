# 05 - Jinja2 Filters

## Useful Filters
```yaml
# Default value fallback
db_host: "{{ custom_db_host | default('localhost') }}"

# JSON conversion
payload: "{{ my_dict | to_json }}"

# Combine dictionaries
merged: "{{ dict_a | combine(dict_b) }}"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [04 - Jinja2 Templating](./04-Jinja2-Templating-Syntax-Filters-and-Control-Structures.md) | [README](./README.md) | [06 - Variable Scoping](./06-Variable-Scoping-Play-Host-Role-and-Extra-Vars.md) |
