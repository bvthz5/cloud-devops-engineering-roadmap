# 10 — Script Arguments and Getopts

## 1. Positional Parameters
- `$0`: Script name.
- `$1 .. $9`, `${10}`: Arguments.
- `$#`: Argument count.
- `$@`: Separate quoted arguments array.

## 2. Option Parsing with `getopts`
```bash
while getopts "e:v" opt; do
  case "$opt" in
    e) ENV="$OPTARG" ;;
    v) VERBOSE=true ;;
  esac
done
```
