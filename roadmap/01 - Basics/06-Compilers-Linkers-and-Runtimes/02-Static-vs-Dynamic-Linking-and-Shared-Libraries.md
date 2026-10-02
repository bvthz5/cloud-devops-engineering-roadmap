# 02 — Static vs Dynamic Linking and Shared Libraries

Understanding how binary executables resolve external library functions dictates how you package and deploy container images.

---

## 1. Comparison: Static vs Dynamic Linking

| Feature | Static Linking (`.a` / Archive) | Dynamic Linking (`.so` / Shared Object) |
| :--- | :--- | :--- |
| **When Linked** | At compile time by `ld` | At runtime by `ld-linux.so` |
| **Binary Size** | Larger (copies library code directly into binary) | Small (contains only symbol stubs) |
| **Dependencies** | **Zero runtime dependencies** (runs anywhere) | Requires matching `.so` files installed on host |
| **RAM Usage** | Multiple running apps have duplicate code in RAM | Linux kernel shares a single copy of `.so` in RAM |
| **Security Updates**| Must recompile binary to apply library patches | Updating host `.so` updates all apps automatically |
| **Container Use**| **Ideal for Docker `FROM scratch`** | Requires full distro base image (Debian/Ubuntu) |

---

## 2. Inspecting Dependencies with `ldd`

`ldd` lists the dynamic shared libraries required by an executable:

```bash
ldd /usr/bin/curl
```
**Example output:**
```text
linux-vdso.so.1 (0x00007ffca15fc000)
libcurl.so.4 => /lib/x86_64-linux-gnu/libcurl.so.4 (0x00007f3c21c00000)
libssl.so.3 => /lib/x86_64-linux-gnu/libssl.so.3 (0x00007f3c21b00000)
libc.so.6 => /lib/x86_64-linux-gnu/libc.so.6 (0x00007f3c21800000)
/lib64/ld-linux-x86-64.so.2 (0x00007f3c21d00000)
```
If a required shared library is missing from the system paths, the binary crashes instantly with:
`error while loading shared libraries: libssl.so.3: cannot open shared object file: No such file or directory`.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [01 - The Build Pipeline](./01-The-Build-Pipeline-Source-to-Machine-Code.md) | [README](./README.md) | [03 - glibc vs musl libc](./03-C-Standard-Libraries-glibc-vs-musl.md) |
