# 01 - Computer Architecture and CPU Fundamentals

---

## 1. Computer Fundamentals

At its foundational level, a computer is a general-purpose electronic information processing system designed to accept arbitrary raw input, apply deterministic algorithmic transformations in accordance with stored instructions, store intermediate and persistent states, and emit formatted output.

### The Unified Information Processing Pipeline

```text
Input Devices               Processing Engine              Memory & Storage            Output Devices
┌──────────────────┐       ┌──────────────────┐       ┌──────────────────────┐       ┌──────────────────┐
│ Keyboard, Mouse  │ ────> │ CPU / Micro-     │ <───> │ Primary (RAM, Cache) │ ────> │ Display Monitor  │
│ Network Packets  │       │ controller       │       │ Persistent (NVMe/SSD)│       │ Network Sockets  │
│ Storage Devices  │       │ Execution Units  │       │ Cloud Object Storage │       │ Storage Writes   │
└──────────────────┘       └──────────────────┘       └──────────────────────┘       └──────────────────┘
```

In Cloud and DevOps infrastructure, computers exist in diverse physical and virtual form-factors: bare-metal rack servers (1U/2U/4U chassis), Virtual Machines (AWS EC2, GCP Compute Engine, Azure VMs), Container Nodes (Kubernetes Worker Nodes), and Edge computing appliances.

---

## 2. Computer Architecture & The Classical Von Neumann Model

Computer architecture represents the blueprint defining how CPU, memory, and peripheral buses communicate. Modern general-purpose computing is fundamentally built upon the **Von Neumann Architecture** (1945), characterized by a central processor, an I/O mechanism, and a **shared memory space holding both program instructions and runtime data**.

```text
                               ┌────────────────────────────────┐
                               │       VON NEUMANN SYSTEM       │
                               └────────────────┬───────────────┘
                                                │
                 ┌──────────────────────────────┼──────────────────────────────┐
                 │ System Bus                   │ Control & Address Bus        │ Data Bus
                 ▼                              ▼                              ▼
        ┌──────────────────┐           ┌──────────────────┐           ┌──────────────────┐
        │       CPU        │           │  Shared Memory   │           │    I/O System    │
        │ ┌──────────────┐ │           │  (RAM / DRAM)    │           │ (Disks, Network, │
        │ │Control Unit  │ │ <───────> │ ┌──────────────┐ │ <───────> │  Console Devices│
        │ ├──────────────┤ │           │ │ Instructions │ │           │                  │
        │ │     ALU      │ │           │ ├──────────────┤ │           │                  │
        │ ├──────────────┤ │           │ │ Runtime Data │ │           │                  │
        │ │  Registers   │ │           │ └──────────────┘ │           │                  │
        │ └──────────────┘ │           └──────────────────┘           └──────────────────┘
        └──────────────────┘
```

> **The Von Neumann Bottleneck:** Because instructions and data must traverse the exact same shared bus, system throughput is fundamentally constrained by the bandwidth of the memory bus. Modern architectures mitigate this bottleneck via multi-level CPU on-die caches (L1, L2, L3) and prefetching logic.

---

## 3. The CPU (Central Processing Unit)

The Central Processing Unit is the primary execution engine. It carries out program logic by fetching binary opcodes from memory, decoding their operational meaning, retrieving referenced operands, performing requested arithmetic or logical transformations, and updating system state.

### Modern Multi-Core Processor Topography

```text
┌────────────────────────────────────────────────────────────────────────┐
│ CPU Socket / Package                                                   │
│ ┌───────────────────────────┐         ┌───────────────────────────┐    │
│ │ Core 0                    │         │ Core 1                    │    │
│ │ ┌─────┐ ┌─────┐ ┌───────┐ │         │ ┌─────┐ ┌─────┐ ┌───────┐ │    │
│ │ │ CU  │ │ ALU │ │ Regs  │ │         │ │ CU  │ │ ALU │ │ Regs  │ │    │
│ │ └─────┘ └─────┘ └───────┘ │         │ └─────┘ └─────┘ └───────┘ │    │
│ │ ┌───────────────────────┐ │         │ ┌───────────────────────┐ │    │
│ │ │ L1 Instruction Cache  │ │         │ │ L1 Instruction Cache  │ │    │
│ │ ├───────────────────────┤ │         │ ├───────────────────────┤ │    │
│ │ │ L1 Data Cache         │ │         │ │ L1 Data Cache         │ │    │
│ │ └───────────────────────┘ │         │ └───────────────────────┘ │    │
│ │ ┌───────────────────────┐ │         │ ┌───────────────────────┐ │    │
│ │ │ L2 Cache (Dedicated)  │ │         │ │ L2 Cache (Dedicated)  │ │    │
│ │ └───────────────────────┘ │         │ └───────────────────────┘ │    │
│ └─────────────┬─────────────┘         └─────────────┬─────────────┘    │
│               │                                     │                  │
│               ▼                                     ▼                  │
│   ┌───────────────────────────────────────────────────────────────┐    │
│   │             Shared L3 Cache (Last Level Cache - LLC)           │    │
│   └───────────────────────────────┬───────────────────────────────┘    │
│                                   │ Integrated Memory Controller (IMC) │
└───────────────────────────────────┼────────────────────────────────────┘
                                    ▼
                          System Physical Memory (DDR4 / DDR5 DRAM)
```

---

## 4. The Control Unit (CU)

The Control Unit acts as the central conductor of the processor. It does not compute math or store application data; instead, it coordinates the entire execution pipeline.

### Core Responsibilities of the Control Unit
1. **Instruction Fetching:** Reads the memory address held in the Program Counter (PC) and loads the target opcode into the Instruction Register (IR).
2. **Instruction Decoding:** Deconstructs the machine code into functional components (Operation Code, Source Register, Destination Register, Memory Offset).
3. **Control Signal Generation:** Broadcasts electrical micro-signals across the internal bus to instruct the ALU, Registers, and Memory controllers on how to route data.
4. **Program Flow Redirection:** Updates the Program Counter when conditional branch instructions (`jmp`, `je`, `jne`, `call`, `ret`) occur.

---

## 5. The Arithmetic Logic Unit (ALU)

The ALU is the mathematical and logical workforce of the processor. When the Control Unit decodes an instruction requiring computation or comparison, operands are routed directly into the ALU input ports.

### ALU Functional Spectrum
- **Arithmetic Operations:** Integer addition (`ADD`), subtraction (`SUB`), multiplication (`MUL`), division (`DIV`), increment, and negation.
- **Bitwise Logic Operations:** `AND`, `OR`, `XOR`, `NOT`.
- **Bit Shifting & Rotation:** Logical Shift Left/Right (`SHL`, `SHR`), Arithmetic Shift Right (`SAR` preserving sign bit), Bit Rotation.
- **Relational Comparisons:** Subtraction without storing the result, setting condition flags (Zero Flag `ZF`, Sign Flag `SF`, Overflow Flag `OF`, Carry Flag `CF`) inside the CPU Status Register.

---

## 6. CPU Registers

Registers are the fastest accessible storage locations in the entire computer hierarchy, residing directly inside the execution core and accessible within a single clock cycle (sub-nanosecond latency).

| Register Category | Typical Size (x86-64) | Examples | Functional Purpose |
|---|---|---|---|
| **Instruction Pointer** | 64 bits | `RIP` (x86_64), `PC` (ARM64) | Holds the memory address of the next instruction. |
| **Stack Management** | 64 bits | `RSP` (Stack Pointer), `RBP` (Base Pointer) | Tracks call stack boundaries and local stack frames. |
| **Status / Flags** | 64 bits | `RFLAGS` (x86_64), `CPSR` (ARM) | Holds conditional flags (ZF, CF, SF, OF, Interrupt Enable). |
| **General Purpose** | 64 bits | `RAX`, `RBX`, `RCX`, `RDX`, `RSI`, `RDI`, `R8`–`R15` | Holds working variables, loop counters, function arguments. |
| **SIMD / Vector** | 128 / 256 / 512 bits | `XMM0`–`XMM15`, `YMM`, `ZMM` | Parallel vector math (AVX-512, NEON) for ML/Crypto. |

---

## 7. Program Counter (PC / RIP)

The Program Counter holds the memory address of the **next instruction** scheduled for execution. 
- During sequential execution, the hardware automatically increments the PC by the byte length of the fetched instruction.
- When jumps, function calls (`call`), system calls (`syscall`), or interrupts occur, the PC is overwritten with the target address of the new code block.

---

## 8. Instruction Register (IR)

The Instruction Register holds the bit-pattern of the current instruction immediately after it is fetched from memory or L1 cache. The decoder hardware reads directly from the IR to decompose the instruction into:
1. **Opcode (Operation Code):** Specifies the action (e.g., move memory, add integer).
2. **Addressing Mode & Operands:** Specifies whether data resides in immediate values, registers, or an indexed memory offset.

---

## 9. Stack Pointer (SP / RSP)

The Stack Pointer holds the memory address of the current top of the call stack in RAM.
- In x86-64 and ARM architectures, the stack **grows downward** (from high memory addresses to low memory addresses).
- When a `PUSH` instruction executes, `RSP` decrements by 8 bytes, and the data is written to `[RSP]`.
- When a `POP` instruction executes, data is read from `[RSP]`, and `RSP` increments by 8 bytes.
- Corrupting or overrunning the Stack Pointer leads to segmentation faults (`SIGSEGV`) or stack overflow security vulnerabilities.

---

## 10. CPU Cores

A CPU core is a completely self-contained physical execution unit. A modern 16-core CPU contains 16 separate ALUs, Control Units, and Register sets.
- **Multi-Core Scaling:** Enables true simultaneous physical parallelism. If a system runs 16 worker threads across 16 physical cores, each thread executes concurrently without time-slicing.
- **Cloud Implication:** Cloud virtual machine sizes (e.g., `c6i.4xlarge`, `t4g.xlarge`) define their compute capacity in terms of **vCPUs**, which map to physical cores or hyper-threaded execution threads.

---

## 11. CPU Threads (Hardware Threads vs SMT)

A hardware thread represents an execution pipeline exposed to the operating system.
- **Simultaneous Multithreading (SMT / Hyper-Threading):** Duplicates the architectural register state (PC, general-purpose registers) while sharing the heavy physical execution units (ALU, FPUs, Caches).
- **vCPU Definition in Cloud:** In AWS, Azure, and GCP, **1 vCPU is typically 1 hardware hyper-thread**, not an entire physical core. A 2-vCPU instance usually shares a single physical CPU core.

```text
Physical Core
┌────────────────────────────────────────────────────────┐
│ Shared Execution Units (ALU, FPU, Branch Predictor)    │
├───────────────────────────┬────────────────────────────┤
│ Architectural State 0     │ Architectural State 1      │
│ (Regs, PC, Stack Pointer) │ (Regs, PC, Stack Pointer)  │
└─────────────┬─────────────┴─────────────┬──────────────┘
              ▼                           ▼
       OS Sees: vCPU 0             OS Sees: vCPU 1
```

---

## 12. CPU Clock Speed

Clock speed (measured in gigahertz, GHz) defines the fundamental frequency of the internal crystal oscillator governing state transitions.
- A **3.0 GHz** processor executes **$3 \times 10^9$ clock ticks per second**.
- **The Performance Equation:**
$$\text{Performance} = \text{Clock Frequency} \times \text{Instructions Per Cycle (IPC)} \times \text{Core Count}$$
- Clock frequency alone is a misleading metric; an modern 2.8 GHz ARM Neoverse or Intel Sapphire Rapids core easily outperforms an older 4.0 GHz processor due to vastly superior IPC and cache architecture.

---

## 13. The Complete CPU Instruction Cycle

Every machine instruction executed by your server passes through five fundamental pipeline phases:

```text
┌──────────────┐      ┌──────────────┐      ┌──────────────┐      ┌──────────────┐      ┌──────────────┐
│ 1. FETCH     │ ───> │ 2. DECODE    │ ───> │ 3. EXECUTE   │ ───> │ 4. MEMORY    │ ───> │ 5. WRITEBACK │
│ Read from PC │      │ Parse Opcode │      │ ALU Compute  │      │ Read/Write   │      │ Commit result│
│ into IR      │      │ & Operands   │      │ Branch test  │      │ RAM / Cache  │      │ to Register  │
└──────────────┘      └──────────────┘      └──────────────┘      └──────────────┘      └──────────────┘
```

1. **Fetch:** The Control Unit reads the instruction at address `[PC]` from L1 Instruction Cache into the Instruction Register.
2. **Decode:** The instruction decoder splits the opcode from arguments and schedules pipeline resources.
3. **Execute:** The ALU computes the arithmetic result or checks conditional flags.
4. **Memory Access (MEM):** If the instruction reads or writes RAM (`mov [rax], rbx`), data passes through the load/store unit.
5. **Writeback (WB):** The final computational result is committed back into the target destination register.

---

| Previous | Index | Next |
| :--- | :---: | ---: |
| [← Start (Roadmap Home)](../../../README.md) | [Index](../../../README.md) | [02 - ISA x86 ARM RISCV and Microarchitecture →](./02-ISA-x86-ARM-RISCV-and-Microarchitecture.md) |
