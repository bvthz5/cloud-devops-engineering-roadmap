# 05 — Compiling and Installing Software from Source

---

## 1. Why Compile Software from Source in DevOps?

While package managers (`apt`, `dnf`, `apk`) should handle 95% of your software installations, compiling from source is mandatory when:
1. **Enabling Custom Compile-Time Modules:** E.g., compiling Nginx with custom third-party modules (like OpenTracing, ModSecurity WAF, or Brotli compression) that are not bundled into distro packages.
2. **Bleeding-Edge Versions:** Deploying a software release that is not yet packaged by Ubuntu or RHEL.
3. **Hardware Architecture Tuning:** Using specialized compiler flags (e.g., `-march=native -O3`) to optimize CPU instruction pipelines for machine learning or high-frequency trading.

---

## 2. Essential Compiler Toolchains

Before compiling C/C++ code, install the foundational compiler packages:

```bash
# On Ubuntu / Debian:
sudo apt update && sudo apt install -y build-essential cmake autoconf pkg-config libssl-dev

# On RHEL / Rocky Linux:
sudo dnf groupinstall -y "Development Tools"
sudo dnf install -y cmake openssl-devel

# On Alpine Linux:
apk add --no-cache build-base cmake openssl-dev
```

---

## 3. The Classic GNU Autotools Pipeline

Most open-source Unix software follows the three-stage build pipeline:

```text
[ Source Tarball (.tar.gz) ]
             │
             ▼ Unpack
        tar -xzf app-1.0.tar.gz
             │
             ▼ 1. Configure System & Compiler Options
        ./configure --prefix=/usr/local --with-ssl
             │  (Generates 'Makefile' tailored to local hardware)
             ▼
        make -j$(nproc)
             │  (Invokes gcc/clang to compile source code into binaries)
             ▼
        sudo make install
                (Copies binaries to /usr/local/bin, configs to /usr/local/etc)
```

### Detailed Breakdown of the Steps:

### Step 1: `./configure`
- Executes a shell script that inspects the local system for required C header files (`.h`), libraries (`.so`), and compiler features.
- Critical Flag: **`--prefix=/opt/myapp`**  
  *Always specify a custom prefix directory!* If you do not specify a prefix, files default to `/usr/local/`, scattering binaries, headers, and libraries across multiple folders, making uninstallation nearly impossible.

### Step 2: `make`
- Reads the generated `Makefile` and coordinates compilation.
- **Speed Tip:** Run parallel builds using all available CPU cores:
  ```bash
  make -j$(nproc)
  ```

### Step 3: `make install`
- Copies compiled binaries, configuration files, and man pages into the directory specified by `--prefix`.

---

## 4. Shared Libraries and `ldconfig`

When you compile and install custom libraries, programs may fail to launch with:
`error while loading shared libraries: libcustom.so.1: cannot open shared object file: No such file or directory`.

The Linux dynamic linker (`ld.so`) caches known library paths in `/etc/ld.so.cache`. If you install libraries into `/usr/local/lib`:

```bash
# 1. Register custom library path in /etc/ld.so.conf.d/
echo "/usr/local/lib" | sudo tee /etc/ld.so.conf.d/custom-libs.conf

# 2. Rebuild the dynamic linker runtime cache
sudo ldconfig

# 3. Verify the library is visible to the system
ldconfig -p | grep libcustom
```

---

## 5. Clean Uninstallation: The `checkinstall` Alternative

The major drawback of `sudo make install` is that it **completely bypasses your operating system's package manager**. If you later want to uninstall the software, running `make uninstall` frequently fails because the developer did not write an uninstall target in the Makefile!

**`checkinstall`** monitors the `make install` step, builds a temporary `.deb` or `.rpm` package, and registers it with `dpkg` or `rpm`:

```bash
# Install checkinstall on Ubuntu:
sudo apt install -y checkinstall

# Run checkinstall instead of 'make install':
sudo checkinstall --pkgname=custom-nginx --pkgversion=1.24.0 --nodoc
```
*Result:* The compiled software is installed, but it is cleanly registered in the package database! You can remove it at any time using:
```bash
sudo apt remove custom-nginx
```

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← 04 - APK Package Manager Alpine Linux and Containers](./04-APK-Package-Manager-Alpine-Linux-and-Containers.md) | [Index](../../../README.md) | [06 - Automated Security Updates and Patching →](./06-Automated-Security-Updates-and-Patching.md) |
