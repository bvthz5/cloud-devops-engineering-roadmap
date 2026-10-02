# 02 - Testing Bash Scripts with Bats and ShellCheck

## 1. Static Analysis with ShellCheck

ShellCheck is a static analysis tool that parses Bash scripts into an Abstract Syntax Tree (AST) to detect subtle bugs, syntax pitfalls, and security vulnerabilities without running the code.

```bash
# Install ShellCheck
sudo apt-get install -y shellcheck

# Run ShellCheck across repository
shellcheck scripts/*.sh
```

### Common ShellCheck Codes Prevented in Production
- **SC2086**: Double quote to prevent globbing and word splitting (`rm -rf $DIR` vs `rm -rf "$DIR"`).
- **SC2181**: Check exit code directly with `if mycmd; then` rather than inspecting `$?`.
- **SC2154**: Referenced variable was assigned in an unreachable branch or misspelled.
- **SC2046**: Quote command substitutions to prevent space splitting (`rm $(find . -name "*.tmp")`).

---

## 2. Bats: Bash Automated Testing System

Bats is a TAP-compliant (Test Anything Protocol) testing framework for Bash. It executes test cases in isolated subshells.

### 2.1 Example Script Under Test (`backup.sh`)
```bash
#!/usr/bin/env bash
set -euo pipefail

backup_dir() {
    local src="$1"
    local dest="$2"

    if [[ ! -d "$src" ]]; then
        echo "Error: Source directory does not exist" >&2
        return 1
    fi

    mkdir -p "$dest"
    tar -czf "$dest/archive.tar.gz" -C "$src" .
    echo "Backup completed successfully"
}
```

### 2.2 Writing Bats Test Suite (`test_backup.bats`)
```bash
#!/usr/bin/env bats

setup() {
    # Executed before each @test
    TEST_TEMP_DIR="$(mktemp -d)"
    SRC_DIR="$TEST_TEMP_DIR/src"
    DEST_DIR="$TEST_TEMP_DIR/dest"
    mkdir -p "$SRC_DIR"
    echo "test-data" > "$SRC_DIR/file.txt"
    
    # Source the script function
    source ./backup.sh
}

teardown() {
    # Executed after each @test
    rm -rf "$TEST_TEMP_DIR"
}

@test "backup_dir succeeds when source directory exists" {
    run backup_dir "$SRC_DIR" "$DEST_DIR"
    
    [ "$status" -eq 0 ]
    [[ "$output" =~ "Backup completed successfully" ]]
    [ -f "$DEST_DIR/archive.tar.gz" ]
}

@test "backup_dir fails and exits 1 when source directory is missing" {
    run backup_dir "$TEST_TEMP_DIR/nonexistent" "$DEST_DIR"
    
    [ "$status" -eq 1 ]
    [[ "$output" =~ "Error: Source directory does not exist" ]]
}
```

```bash
# Run the test suite
bats test_backup.bats
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - Testing Pyramid](./01-The-Testing-Pyramid-for-Infrastructure-and-DevOps.md) | [README](./README.md) | [03 - Python Testing with pytest and Moto](./03-Python-Unit-and-Integration-Testing-pytest-and-Moto.md) |
