# 10 — Hands-On Practice Labs: Compilers & Runtimes

---

## Lab 1: Comparing Static vs Dynamic Compilation

```bash
# 1. Create a simple C program
cat << 'EOF' > hello.c
#include <stdio.h>
int main() {
    printf("Hello from C Binary!
");
    return 0;
}
EOF

# 2. Compile dynamically (standard default)
gcc hello.c -o hello-dynamic

# 3. Compile statically
gcc -static hello.c -o hello-static

# 4. Compare file sizes and dependencies
ls -lh hello-dynamic hello-static
ldd hello-dynamic
ldd hello-static || echo "Statically linked: No dependencies!"
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 09 - Interview QA](./09-Interview-QA.md) | [Index](../../../README.md) | [11 - MCQ →](./11-MCQ.md) |
