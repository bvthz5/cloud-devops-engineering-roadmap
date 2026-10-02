# 03 - Location Block Matching Priority and Directives

## 1. The 5 Location Modifiers and Precedence

When a request arrives, Nginx determines which `location` block executes using an exact, deterministic evaluation algorithm. It does **NOT** simply pick the first match from top to bottom!

| Modifier | Type | Evaluation Order / Priority | Description |
|---|---|:---:|---|
| `=` | **Exact Match** | **1 (Highest)** | Matches the exact URI string only. Terminates search immediately if matched. |
| `^~` | **Preferential Prefix** | **2** | If matched, stops regex searching. Longest prefix wins. |
| `~` | **Case-Sensitive Regex**| **3** | Evaluated top-to-bottom in order of appearance in config. |
| `~*`| **Case-Insensitive Regex**| **3** | Evaluated top-to-bottom in order of appearance in config. |
| *(none)* | **Standard Prefix** | **4 (Lowest)** | Matches starting string. Continues searching; longest prefix wins if no regex matches. |

```text
Decision Flowchart for Incoming URI:
             [ Incoming Request URI ]
                         │
                         ▼
        Does an Exact Match (`=`) exist?
           ├──► YES: Execute that block immediately. STOP.
           └──► NO
                 │
                 ▼
        Find all Prefix Matches (both `^~` and standard prefix)
        Select the LONGEST matching prefix.
                 │
                 ├──► Does the longest match use `^~`?
                 │       └──► YES: Execute that block immediately. STOP.
                 └──► NO (standard prefix stored as best fallback)
                         │
                         ▼
                 Evaluate Regular Expressions (`~` and `~*`)
                 in strict top-to-bottom file order!
                         ├──► First Regex Matched: Execute that block. STOP.
                         └──► No Regex Matched: Execute stored longest prefix match!
```

---

## 2. Practical Precedence Demonstration

Consider this Nginx configuration:

```nginx
server {
    listen 80;
    server_name example.com;

    # Location A: Exact Match
    location = /images {
        return 200 "Block A: Exact /images\n";
    }

    # Location B: Preferential Prefix Match
    location ^~ /images/avatars/ {
        return 200 "Block B: Preferential /images/avatars/\n";
    }

    # Location C: Case-Insensitive Regex
    location ~* \.(gif|jpg|jpeg|png)$ {
        return 200 "Block C: Image Regex\n";
    }

    # Location D: Standard Prefix Match
    location /images/ {
        return 200 "Block D: Prefix /images/\n";
    }

    # Location E: Catch-all Fallback
    location / {
        return 200 "Block E: Root Fallback\n";
    }
}
```

### Request Resolution Table
- `GET /images` ➔ **Block A** (Exact match wins immediately).
- `GET /images/avatars/john.png` ➔ **Block B** (`^~ /images/avatars/` is longest prefix and prevents regex evaluation, so Block C is ignored!).
- `GET /images/hero.png` ➔ **Block C** (Prefix `/images/` matches, but regex `~* \.(png)$` has higher priority than standard prefix).
- `GET /images/docs/readme.txt` ➔ **Block D** (Longest prefix match; regex does not match `.txt`).
- `GET /about-us` ➔ **Block E** (Fallback prefix).

---

## 3. `root` vs `alias` Directive Pitfall

One of the most frequent production bugs is confusing `root` and `alias`.

```nginx
# SCENARIO: Serving files from /var/data/static/

# 1. Using ROOT: Appends the FULL request URI to root path!
location /static/ {
    root /var/data; 
    # Request: GET /static/app.js
    # File evaluated: /var/data/static/app.js  <-- Appends /static/
}

# 2. Using ALIAS: REPLACES the matched location prefix with alias path!
location /static/ {
    alias /var/data/; 
    # Request: GET /static/app.js
    # File evaluated: /var/data/app.js         <-- Strips /static/!
}
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [02 - Configuration File Hierarchy](./02-Configuration-File-Hierarchy-and-Contexts.md) | [README](./README.md) | [04 - URL Rewriting & Redirection](./04-URL-Rewriting-Redirection-and-Returns.md) |
