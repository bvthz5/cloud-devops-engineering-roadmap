# 12 - Paths, Wildcards & Shell Expansion Mechanics

The Linux shell performs several layers of expansion on your input command line *before* handing arguments to the executing utility.

---

## 1. Shell Wildcards (Globbing)

Wildcards allow you to match patterns of filenames without typing explicit names.

```bash
# 1. Asterisk '*': Matches ZERO or more characters:
$ ls *.log            # Matches 'app.log', 'error.log', '.log'
$ rm -f test_*        # Matches 'test_1', 'test_backup', 'test_'

# 2. Question Mark '?': Matches EXACTLY ONE character:
$ ls log_?.txt        # Matches 'log_1.txt', 'log_A.txt' (Not 'log_10.txt')

# 3. Character Sets '[...]': Matches any single character enclosed in brackets:
$ ls app_v[123].py    # Matches 'app_v1.py', 'app_v2.py', 'app_v3.py'
$ ls [a-z]*.txt       # Matches any file starting with a lowercase letter

# 4. Negated Sets '[!...]' or '[^...]': Matches characters NOT enclosed:
$ ls log_[!0-9].txt   # Matches 'log_a.txt', but excludes 'log_1.txt'
```

---

## 2. Brace Expansion (`{...}`)

Brace expansion is performed by the shell **before** filename globbing. It generates arbitrary lists of strings.

### Sequence & Range Expansion:
```bash
# Generate numbered sequences:
$ echo file_{1..5}.txt
file_1.txt file_2.txt file_3.txt file_4.txt file_5.txt

# Zero-padded sequence:
$ touch log_{01..05}.txt
# Creates: log_01.txt, log_02.txt, log_03.txt, log_04.txt, log_05.txt
```

### List Expansion:
```bash
# Scaffold a complex directory tree in 1 line:
$ mkdir -p microservice/{cmd,pkg,api/v1,deploy/{helm,k8s}}

# The DevOps Quick Backup Shortcut:
$ cp nginx.conf{,.bak}
# Expands to: cp nginx.conf nginx.conf.bak
```

---

## 3. Escaping Special Characters & Quoting Rules

If a filename contains spaces or special characters (`*`, `$`, `?`, `!`), you must prevent the shell from interpreting them.

```bash
# 1. Backslash '\': Escapes the single immediate character following it:
$ rm My\ Document.pdf

# 2. Single Quotes '...': Preserves LITERAL value of EVERY character inside:
$ echo '$VAR * ?'
$VAR * ?   # (No variable expansion, no wildcard matching!)

# 3. Double Quotes "...": Preserves literal value except for '$', '`', and '\':
$ VAR="Linux"
$ echo "Hello $VAR"
Hello Linux   # (Variable is expanded!)
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [11 - All Flags Cheat Sheet](./11-All-Flags-Cheat-Sheet.md) | [README](./README.md) | [13 - Permissions and Ownership](./13-Permissions-and-Ownership.md) |
