# 30 — Complete Command Cheat Sheet

## Syntax Cheat Sheet

### Variables & Expressions
```bash
VAR="value"                       # Assignment (no spaces!)
echo "$VAR"                       # Variable expansion
export VAR                        # Export to child subshells
readonly VAR="constant"           # Immutable constant
NUM=$(( 10 + 5 ))                # Integer arithmetic
PCT=$(echo "scale=2; 5/2" | bc)  # Floating-point arithmetic
```

### Tests & Conditionals
```bash
[[ -f "$FILE" ]]                  # Regular file test
[[ -d "$DIR" ]]                   # Directory test
[[ -z "$STR" ]]                   # String empty test
[[ -n "$STR" ]]                   # String non-empty test
[[ "$A" == "$B" ]]                # String equality test
[[ "$N1" -gt "$N2" ]]             # Numeric greater than
```

### Parameter Expansion
```bash
${VAR:-default}                   # Fallback if unset
${VAR:=default}                   # Assign fallback if unset
${VAR:?error message}             # Fatal error if unset
${#VAR}                           # String length
${VAR#pattern}                    # Strip shortest prefix match
${VAR##pattern}                   # Strip longest prefix match
${VAR%pattern}                    # Strip shortest suffix match
${VAR%%pattern}                   # Strip longest suffix match
```

### Text Processing Pipelines
```bash
grep -r "pattern" /path           # Recursive search
sed -i 's/old/new/g' file.txt    # In-place substitution
awk '{print $1}' log.txt          # Print 1st column
sort -nr                          # Numeric reverse sort
uniq -c                           # Count unique occurrences
```
---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [29 - Quick Revision Notes](./29-Quick-Revision-Notes.md) | [README](./README.md) | [31 - Production Scripting Checklist](./31-Production-Scripting-Checklist.md) |
