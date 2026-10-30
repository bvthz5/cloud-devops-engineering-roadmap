# 04 - Jinja2 Templating Syntax, Filters & Control Structures

In modern infrastructure automation, configuration files (such as Nginx configurations, HAProxy backend pools, systemd unit definitions, and Prometheus targets) must adapt dynamically to changing server IP addresses, cluster sizes, and environment variables. Ansible solves this by embedding the Python-based **Jinja2** templating engine directly into its execution core.

Using the `ansible.builtin.template` module, Ansible renders dynamic source templates (typically ending in `.j2`) on the control node before transferring the finalized static configuration to the managed node.

---

## 🏗️ 1. Jinja2 Core Syntax Delimiters

Jinja2 distinguishes between variable interpolation, logical execution, and comments using three fundamental delimiter pairs:

```text
┌────────────────────────────────────────────────────────────────────────────────────────┐
│                                 JINJA2 DELIMITER SYSTEM                                │
│                                                                                        │
│   {{ expression }}   ──► Output Interpolation (Variables, math, filters)               │
│                          Example: server_name {{ ansible_facts['fqdn'] }};             │
│                                                                                        │
│   {% statement %}    ──► Control Structures (Loops, conditionals, variable set)        │
│                          Example: {% if enable_ssl %} listen 443 ssl; {% endif %}      │
│                                                                                        │
│   {# comment #}      ──► Template Comments (Ignored by engine, never written to disk)  │
│                          Example: {# Managed by Ansible - Do not edit manually #}      │
└────────────────────────────────────────────────────────────────────────────────────────┘
```

### Whitespace Trimming Control (`-`)

By default, Jinja2 preserves whitespace and line breaks around control statements. In configuration files sensitive to newlines or indentation (such as YAML, Python, or Apache configurations), this can produce unwanted blank lines.

Adding a hyphen (`-`) inside the delimiter instructs Jinja2 to strip whitespace:
- `{%-` strips leading whitespace before the block.
- `-%}` strips trailing whitespace and newlines after the block.
- `{{-` strips leading whitespace before an expression.
- `-}}` strips trailing whitespace after an expression.

```jinja2
# Without whitespace trimming: produces 5 empty lines
{% for host in groups['webservers'] %}
server {{ host }};
{% endfor %}

# With whitespace trimming: clean, compact output
{% for host in groups['webservers'] -%}
server {{ host }};
{% endfor -%}
```

---

## 🔁 2. Control Structures: Conditionals & Loops

### 1. Conditionals (`if`, `elif`, `else`)

Conditionals allow templates to evaluate facts or variables to render environment-specific blocks:

```jinja2
server {
    listen {{ http_port | default(80) }};
    server_name {{ server_domain }};

{% if enable_tls | default(false) | bool %}
    listen 443 ssl http2;
    ssl_certificate {{ ssl_cert_path }};
    ssl_certificate_key {{ ssl_key_path }};
    ssl_protocols TLSv1.2 TLSv1.3;
{% elif enforce_https_redirect | default(false) | bool %}
    return 301 https://$host$request_uri;
{% else %}
    # Plain HTTP listener active
{% endif %}
}
```

### 2. Loops (`for`) & Loop Context Variables

Loops iterate over lists, dictionaries, or host groups. Inside a `for` block, Jinja2 provides special loop context variables:

| Loop Variable | Description |
| :--- | :--- |
| `loop.index` | Current iteration number starting from 1. |
| `loop.index0` | Current iteration number starting from 0 (zero-indexed). |
| `loop.first` | Boolean `True` if this is the first iteration. |
| `loop.last` | Boolean `True` if this is the final iteration. |
| `loop.length` | Total number of items in the iterable sequence. |

```jinja2
upstream backend_cluster {
{% for host in groups['app_servers'] %}
    server {{ hostvars[host]['ansible_facts']['default_ipv4']['address'] }}:{{ app_port }}{% if loop.first %} primary{% endif %};
{% endfor %}
}
```

---

## 🔬 3. Essential Enterprise Jinja2 Filters

Filters transform variables using the pipe operator (`|`). They can be chained sequentially: `{{ variable | filter1 | filter2 }}`.

### 1. Default & Fallback Filters
```jinja2
# Provide fallback if variable is undefined
listen_port: {{ app_port | default(8080) }}

# Provide fallback if variable is undefined OR empty string
log_level: {{ app_log_level | default('INFO', true) }}
```

### 2. Data Structure Transformation
```jinja2
# Extract a specific attribute from a list of objects
{{ users | map(attribute='username') | list }}

# Filter list by matching attribute
{{ servers | selectattr('environment', 'equalto', 'production') | list }}

# Exclude list items matching attribute
{{ disks | rejectattr('mount', 'equalto', '/boot') | list }}

# Sort and de-duplicate
{{ ip_addresses | unique | sort }}
```

### 3. Serialization Filters
```jinja2
# Convert complex dictionary to formatted JSON
{{ app_config | to_nice_json(indent=2) }}

# Convert complex dictionary to YAML
{{ k8s_manifest | to_nice_yaml(indent=2) }}
```

### 4. String & Encoding Filters
```jinja2
# Base64 encoding for Kubernetes secrets
api_secret: {{ raw_token | b64encode }}

# SHA256 checksum calculation
file_hash: {{ license_key | hash('sha256') }}

# Regex search and replacement
clean_name: {{ raw_hostname | regex_replace('[^a-zA-Z0-9]', '_') }}
```

### 5. Network & IP Address Filters (via `ansible.utils`)
```jinja2
# Check if an IP address is valid IPv4
{% if my_ip | ansible.utils.ipv4 %} valid {% endif %}

# Calculate subnet network address from CIDR
network_addr: {{ '192.168.1.50/24' | ansible.utils.ipaddr('network') }}
```

---

## 📄 4. Production Example: `templates/nginx-reverse-proxy.conf.j2`

Here is an enterprise-grade Nginx reverse proxy template utilizing loops, conditions, facts, and whitespace trimming:

```jinja2
{# ========================================================================= #}
{# Managed by Ansible. Handled by role: nginx_proxy. DO NOT EDIT MANUALLY.   #}
{# Node: {{ ansible_facts['fqdn'] }} | Generated: {{ ansible_facts['date_time']['iso8601'] }} #}
{# ========================================================================= #}

upstream {{ proxy_upstream_name }} {
    least_conn;
{% for backend in groups[proxy_upstream_group] %}
    server {{ hostvars[backend]['ansible_facts']['default_ipv4']['address'] }}:{{ proxy_backend_port }} max_fails=3 fail_timeout=10s;
{% endfor %}
    keepalive 32;
}

server {
    listen {{ proxy_listen_port | default(80) }};
    server_name {{ proxy_server_name | default(ansible_facts['fqdn']) }};

{% if proxy_enable_tls | default(false) | bool %}
    listen 443 ssl http2;
    ssl_certificate {{ proxy_tls_cert_path }};
    ssl_certificate_key {{ proxy_tls_key_path }};
    ssl_session_timeout 1d;
    ssl_session_cache shared:SSL:10m;
    ssl_protocols TLSv1.2 TLSv1.3;
{% endif %}

    location / {
        proxy_pass http://{{ proxy_upstream_name }};
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        
        proxy_connect_timeout {{ proxy_timeout | default(60) }}s;
        proxy_read_timeout {{ proxy_timeout | default(60) }}s;
    }

    # Health check endpoint
    location /healthz {
        access_log off;
        return 200 'OK';
        add_header Content-Type text/plain;
    }
}
```

---

## 🎯 5. Deploying Templates with Pre-Flight Validation

A critical enterprise feature of the `ansible.builtin.template` module is the `validate` parameter. Ansible renders the template to a temporary location on the managed node and executes a validation command before replacing the live configuration. If the validation command fails, Ansible aborts the task and preserves the existing working configuration.

```yaml
- name: Deploy and validate Nginx configuration template
  ansible.builtin.template:
    src: templates/nginx-reverse-proxy.conf.j2
    dest: /etc/nginx/conf.d/reverse-proxy.conf
    owner: root
    group: root
    mode: '0644'
    # %s is replaced by the path to the candidate temporary file
    validate: 'nginx -t -c %s'
  notify: Reload Nginx
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 03 - Registering Variables and Task Output Capture](./03-Registering-Variables-and-Task-Output-Capture.md) | [Index](../../../README.md) | [05 - Built in Jinja2 Filters and Custom Filters →](./05-Built-in-Jinja2-Filters-and-Custom-Filters.md) |
