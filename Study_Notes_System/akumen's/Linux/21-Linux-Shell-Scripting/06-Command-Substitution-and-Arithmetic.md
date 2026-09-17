# 06 — Command Substitution and Arithmetic

## 1. Command Substitution Mechanics
Command substitution captures the stdout stream of a subprocess command and assigns it to a variable or passes it as an argument.

### Modern Syntax: `$(command)` vs Legacy Syntax: `` `command` ``

```bash
# MODERN & PREFERRED Syntax (supports nesting and clean readability)
CURRENT_DATE="$(date +%Y-%m-%d)"
CPU_COUNT="$(nproc)"
KERNEL_VERSION="$(uname -r)"

# Legacy Backticks Syntax (DO NOT USE in modern scripts; nesting requires complex escaping)
LEGACY_DATE=`date +%Y-%m-%d`

# Nesting command substitution example:
PARENT_DIR="$(basename "$(dirname "$(pwd)")")"
echo "Parent directory name: $PARENT_DIR"
```

## 2. Integer Arithmetic in Bash
Bash natively supports **integer arithmetic only** (64-bit signed integers).

### Method A: Double Parentheses Arithmetic `$(( ... ))` (PREFERRED)
```bash
NUM1=15
NUM2=4

# Addition, Subtraction, Multiplication, Division, Modulo
SUM=$(( NUM1 + NUM2 ))       # 19
DIFF=$(( NUM1 - NUM2 ))      # 11
PRODUCT=$(( NUM1 * NUM2 ))   # 60
QUOTIENT=$(( NUM1 / NUM2 ))  # 3 (integer division, truncates decimals!)
REMAINDER=$(( NUM1 % NUM2 )) # 3

# Increment / Decrement
COUNTER=0
(( COUNTER++ ))              # COUNTER becomes 1
(( COUNTER += 5 ))           # COUNTER becomes 6

echo "Sum: $SUM, Quotient: $QUOTIENT, Counter: $COUNTER"
```

### Method B: `let` Builtin Command
```bash
let "RESULT = 10 + 20"
let "RESULT++"
echo "$RESULT"  # Outputs 31
```

### Method C: Legacy `expr` Tool (Deprecated)
```bash
# Requires spaces around operators!
RESULT=$(expr 10 + 20)
```

## 3. Floating-Point Arithmetic using `bc` (Basic Calculator)
Because Bash integer arithmetic truncates decimal points (`10/4 = 2`), use `bc` for precise floating-point or scientific calculations.

```bash
# Calculate percentage disk space used
TOTAL_MEM=16000
USED_MEM=4200

# Pipe calculation into bc with scale setting (scale defines decimal places)
PERCENTAGE=$(echo "scale=2; ($USED_MEM / $TOTAL_MEM) * 100" | bc)
echo "Memory utilization: ${PERCENTAGE}%"
# Output: Memory utilization: 26.25%

# Floating-point condition checks using bc
HIGH_LOAD=0.85
CURRENT_LOAD=1.24

IS_OVERLOAD=$(echo "$CURRENT_LOAD > $HIGH_LOAD" | bc)

if [[ "$IS_OVERLOAD" -eq 1 ]]; then
  echo "WARNING: Server CPU load ($CURRENT_LOAD) exceeds threshold ($HIGH_LOAD)!"
fi
```
