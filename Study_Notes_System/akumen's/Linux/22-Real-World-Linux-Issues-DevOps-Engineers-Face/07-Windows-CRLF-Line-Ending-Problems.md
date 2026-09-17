# 07 — Windows CRLF Line Ending Problems

## 1. Scenario
A shell script committed from a Windows machine fails on Ubuntu server with the following error:
`bash: ./deploy.sh: /bin/bash^M: bad interpreter: No such file or directory`

```text
Scenario
   ↓
Symptoms: /bin/bash^M bad interpreter, syntax error near unexpected token 'then', unexpected end of file
   ↓
What could cause it? Windows CRLF (\r\n) line endings present in file instead of Unix LF (\n)
   ↓
Diagnostic commands: file script.sh, cat -v script.sh, od -c script.sh | head
   ↓
Find root cause: Non-printable carriage return character \r (0x0D / ^M) appended to line ends
   ↓
Fix / mitigate: Convert file using dos2unix script.sh or sed -i 's/\r$//' script.sh
   ↓
Prevent recurrence: Add .gitattributes file with '* text=auto eol=lf'
```

## 2. Diagnostic Identification

```bash
# 1. Check file encoding type
file deploy.sh
# Output: deploy.sh: POSIX shell script, ASCII text executable, with CRLF line terminators

# 2. View hidden carriage return control characters (^M)
cat -v deploy.sh
# Output: #!/bin/bash^M
```

## 3. Immediate Fix Methods

### Method A: `dos2unix`
```bash
dos2unix deploy.sh
```

### Method B: `sed` Stream Editor
```bash
sed -i 's/\r$//' deploy.sh
```

## 4. Repository Prevention (`.gitattributes`)
Create `.gitattributes` in your repository root directory to force Git to check out files with LF line endings on all systems:

```gitattributes
* text=auto eol=lf
*.sh text eol=lf
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [06 - Permissions & PATH](./06-Permissions-Ownership-and-Path-Resolution-Issues.md) | [README](./README.md) | [08 - Systemd Service Failures](./08-Systemd-Service-Failures-and-Crash-Loops.md) |
