# 01 — Number Systems: Decimal, Binary, Octal, Hexadecimal, and Storage Units

---

## 1. How Computers Understand Information

A computer is an electronic circuit composed of billions of microscopic transistors acting as switches. At the silicon level, digital circuits recognize only two electrical states:
- `0` — Low voltage (Switch OFF)
- `1` — High voltage (Switch ON)

This two-state number system is called **Binary** (Base 2).

```text
┌────────────────────────────────────────────────────────┐
│                   HOW DATA IS STORED                   │
│                                                        │
│   Transistor Voltage:   [ LOW ]       [ HIGH ]         │
│   Binary Digit:            0              1            │
│                            ▲              ▲            │
│                            │              │            │
│   Sequences:         0 1 1 0 1 0 0 1 1 0 1 1 ...       │
│                      │                                 │
│                      ▼                                 │
│             Can represent: Numbers, Text, Photos,      │
│             Videos, Code, and the Linux Kernel!        │
└────────────────────────────────────────────────────────┘
```

---

## 2. Bits vs. Bytes

- **Bit (b):** The smallest unit of digital data. Holds one binary digit (`0` or `1`).
- **Byte (B):** A contiguous group of **8 bits**.

$$\mathbf{1\text{ Byte}} = \mathbf{8\text{ bits}}$$

```text
┌───┐ ┌───┐ ┌───┐ ┌───┐ ┌───┐ ┌───┐ ┌───┐ ┌───┐
│ 1 │ │ 0 │ │ 1 │ │ 1 │ │ 0 │ │ 0 │ │ 1 │ │ 0 │  <── 8 Bits = 1 Byte
└───┘ └───┘ └───┘ └───┘ └───┘ └───┘ └───┘ └───┘
  ▲                                           ▲
  │                                           │
Most Significant Bit (MSB)        Least Significant Bit (LSB)
```

- **Lowercase `b` = Bits:** Used for transmission speeds (e.g., 100 Mbps internet connection).
- **Uppercase `B` = Bytes:** Used for storage and file sizes (e.g., 100 MB file size).

---

## 3. Decimal Units vs. Binary Units (KB vs. KiB, MB vs. MiB, GB vs. GiB)

One of the most frequent points of confusion for beginners and engineers is the difference between **Decimal (SI, Base 10)** and **Binary (IEC, Base 2)** prefixes:

```text
┌──────────────────────────────┬──────────────────────────────┐
│  DECIMAL / SI UNITS (1,000)  │  BINARY / IEC UNITS (1,024)  │
├──────────────────────────────┼──────────────────────────────┤
│ 1 kB  = 1,000 Bytes (10³)    │ 1 KiB = 1,024 Bytes (2¹⁰)    │
│ 1 MB  = 1,000,000 Bytes (10⁶)│ 1 MiB = 1,048,576 Bytes (2²⁰)│
│ 1 GB  = 1,000,000,000 B (10⁹)│ 1 GiB = 1,073,741,824 B (2³⁰)│
│ 1 TB  = 10¹² Bytes           │ 1 TiB = 2⁴⁰ Bytes            │
└──────────────────────────────┴──────────────────────────────┘
```

### 💡 Why does a 500 GB SSD appear as ~466 GiB in Linux?

Hard drive and SSD manufacturers advertise storage using decimal units ($1\text{ GB} = 10^9\text{ bytes}$):

$$\text{Advertised Storage} = 500{,}000{,}000{,}000\text{ bytes}$$

Operating systems like Linux calculate storage capacity in binary units:

$$\text{Real Capacity in GiB} = \frac{500{,}000{,}000{,}000}{1{,}073{,}741{,}824} \approx \mathbf{465.66\text{ GiB}}$$

Your storage is not missing! The operating system is displaying **Gibibytes (GiB)** while labeling it with the decimal name or calculating in powers of 1,024.

---

## 4. How Binary Numbers Work (Base 2)

In decimal (Base 10), each position moving left represents a power of 10 ($10^0, 10^1, 10^2$).  
In binary (Base 2), each position moving left represents a power of 2 ($2^0=1, 2^1=2, 2^2=4, 2^3=8, 2^4=16, \dots$).

### Example: Binary `1011` to Decimal

```text
Binary Bit:     1        0        1        1
Power of 2:    2³       2²       2¹       2⁰
Place Value:    8        4        2        1
Calculation: (1 × 8) + (0 × 4) + (1 × 2) + (1 × 1) = 8 + 0 + 2 + 1 = 11
```

$$\mathbf{1011_2} = \mathbf{11_{10}}$$

### Step-by-Step Conversion: Decimal 13 to Binary

Divide by 2 repeatedly and record the remainder:

| Calculation | Quotient | Remainder | Position |
| :--- | :---: | :---: | :--- |
| $13 \div 2$ | 6 | **1** | LSB (rightmost) |
| $6 \div 2$ | 3 | **0** | |
| $3 \div 2$ | 1 | **1** | |
| $1 \div 2$ | 0 | **1** | MSB (leftmost) |

Read the remainders from **bottom to top**:

$$\mathbf{13_{10}} = \mathbf{1101_2}$$

---

## 5. The Four Core Number Systems in Computer Engineering

| Number System | Base (Radix) | Allowed Digits | Primary Use Case in DevOps & Linux |
| :--- | :---: | :--- | :--- |
| **Binary** | Base 2 | `0, 1` | Physical hardware, bitmasks, network subnet masks. |
| **Octal** | Base 8 | `0, 1, 2, 3, 4, 5, 6, 7` | **Linux file permissions** (`chmod 755`, `chmod 644`). Exactly 3 bits ($2^3=8$). |
| **Decimal** | Base 10 | `0 to 9` | Human everyday counting, port numbers (e.g., 443, 80). |
| **Hexadecimal** | Base 16 | `0-9, A, B, C, D, E, F` | **Memory addresses, MAC addresses, IPv6, SHA256 hashes**. Exactly 4 bits ($2^4=16$). |

### Why Linux Permissions Use Octal (Base 8)

Each Linux permission category has 3 independent binary bits:
- Read (`r`) = 4 ($2^2$)
- Write (`w`) = 2 ($2^1$)
- Execute (`x`) = 1 ($2^0$)

```text
Entity:         Owner (User)          Group               Others
Binary Bits:    r   w   x         r   w   x         r   w   x
Place Values:   4   2   1         4   2   1         4   2   1
Bit Values:     1   1   1         1   0   1         1   0   1
Total:          4 + 2 + 1 = 7     4 + 0 + 1 = 5     4 + 0 + 1 = 5

Resulting Octal Command: chmod 755 script.sh
```

Because 3 bits map 1-to-1 to an octal digit (0 to 7), Linux permissions use Base 8 natively!

---

## 6. How Text is Stored: ASCII, Unicode & UTF-8

Computers cannot store characters like `'A'` directly; they store the numeric binary encoding assigned to each character:

- **ASCII (7-bit, 128 characters):** Standard English letters, digits, and control codes.
  - `'A'` = Decimal 65 = Binary `01000001`
  - `'a'` = Decimal 97 = Binary `01100001`
  - `'0'` = Decimal 48 = Binary `00110000`
- **Unicode (1,114,112 code points):** Universal character set covering every language (Chinese, Arabic, Hindi, Cyrillic, emojis 🚀).
- **UTF-8 (Variable 1 to 4 bytes):** The gold standard of Linux, Git, Docker, and the Web.
  - Backward compatible with ASCII (English letters use 1 byte).
  - Accented letters use 2 bytes, Asian scripts use 3 bytes, Emojis use 4 bytes.

> ⚠️ **DevOps Impact:** Mismatched character encodings cause log corruption, broken JSON parsing, environment variable failures, and invalid CI/CD script execution.

---

## 7. 32-bit vs. 64-bit Architecture

The bit-width defines the register size and memory address space:

```text
┌────────────────────────────────────┬────────────────────────────────────┐
│ 32-BIT ARCHITECTURE (x86 / IA-32)  │ 64-BIT ARCHITECTURE (x86_64 / ARM) │
├────────────────────────────────────┼────────────────────────────────────┤
│ • 32-bit general purpose registers │ • 64-bit general purpose registers │
│ • Addressable RAM: 2³² bytes       │ • Addressable RAM: 2⁶⁴ bytes       │
│ • Maximum RAM: Exactly 4 GB!       │ • Maximum RAM: 16 Exabytes!        │
│ • Rare in modern cloud servers     │ • Universal standard for servers   │
└────────────────────────────────────┴────────────────────────────────────┘
```

### Check Your Linux System Architecture:
```bash
uname -m
# Output: x86_64  (Intel/AMD 64-bit) or aarch64 (ARM 64-bit)

getconf LONG_BIT
# Output: 64
```

> ⚠️ **Common Docker Bug: `exec format error`:** If you build a Docker image on an Apple Silicon Mac (M1/M2/M3, which is ARM64 / `aarch64`) and attempt to run it on an AWS Intel server (`x86_64`), the Linux kernel cannot execute the binary and throws `exec format error`!

---

## 8. Network Bandwidth vs. Data Throughput (Mbps vs. MB/s)

Network speeds are measured in **bits per second (bps)**, while files and Docker images are measured in **bytes (B)**.

$$1\text{ Byte} = 8\text{ bits} \implies \text{Speed in MB/s} = \frac{\text{Speed in Mbps}}{8}$$

### Example: Downloading a 1 GB Docker Image on a 100 Mbps Connection

1. Calculate maximum theoretical download rate:
   $$\frac{100\text{ Mbps}}{8} = \mathbf{12.5\text{ MB/s}}$$
2. Calculate download duration:
   $$\frac{1{,}000\text{ MB}}{12.5\text{ MB/s}} = \mathbf{80\text{ seconds}}$$

Accounting for TCP/IP packet overhead, latency, and TLS encryption, real-world downloads will take ~90–100 seconds.

---

## 9. Essential Inspection Commands Cheat Sheet

| Command | What it Shows | Output Example |
| :--- | :--- | :--- |
| `df -h` | Disk filesystem capacity in human-readable units (GiB) | `/dev/sda1 40G 12G 28G 30% /` |
| `free -h` | RAM and swap usage in human-readable units (GiB) | `Mem: 7.8Gi 2.1Gi 3.4Gi` |
| `ls -lh file.iso` | File size formatted in K, M, or G | `-rw-r--r-- 1 user user 1.2G` |
| `uname -m` | CPU architecture string | `x86_64` or `aarch64` |
| `lsblk` | Block storage devices and partition layout | `sda 500G 0 disk` |
| `echo 'obase=2; 13' \| bc` | Convert decimal number to binary | `1101` |

---

## 10. Quick Revision Quiz

1. **How many bits are in one byte?**
   - A) 2
   - B) 4
   - C) 8 *(Correct)*
   - D) 16

2. **Why does an advertised 500 GB SSD show ~466 GiB in Linux?**
   - A) 34 GB of hardware is damaged
   - B) Manufacturers calculate in decimal ($10^9$), while Linux calculates in binary ($2^{30}$) *(Correct)*
   - C) The Linux kernel takes 34 GB
   - D) Swap space automatically takes 34 GB

3. **Convert decimal 10 to binary:**
   - A) 1001
   - B) 1010 *(Correct - $8 + 0 + 2 + 0 = 10$)*
   - C) 1100
   - D) 1110

4. **What causes the Docker error `exec format error`?**
   - A) The container ran out of memory
   - B) The Dockerfile had a syntax error
   - C) The image was compiled for a different CPU architecture (e.g. ARM64 vs x86_64) *(Correct)*
   - D) The container port was already in use

5. **A 100 Mbps network connection has a theoretical maximum transfer rate of:**
   - A) 100 MB/s
   - B) 50 MB/s
   - C) 12.5 MB/s *(Correct: $100 \div 8$)*
   - D) 8 MB/s

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Prev Module (04-Data-Formats)](../04-Data-Formats-YAML-JSON-XML-TOML/14-Quick-Revision.md) | [Basics Index](../../../README.md) | [02 - Binary Arithmetic and Bitwise Operations →](./02-Binary-Arithmetic-Twos-Complement-and-Bitwise-Operations.md) |
