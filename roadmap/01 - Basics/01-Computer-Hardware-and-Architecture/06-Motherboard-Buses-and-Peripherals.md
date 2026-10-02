# 06 - Motherboards, Buses, GPUs, NICs, DMA & Interrupts

---

## 1. The Motherboard & Chipset Architecture

The motherboard (or system board) provides the physical circuit traces and electrical routing interconnecting the CPU, DRAM modules, storage devices, and expansion cards.

In modern enterprise server architecture, the traditional "Northbridge" memory controller is integrated directly into the physical CPU die (Integrated Memory Controller - IMC). The remaining lower-speed peripheral connections (USB, SATA, legacy I/O, SPI flash) are managed by a consolidated hub known as the **Platform Controller Hub (PCH)** or Chipset.

```text
┌────────────────────────────────────────────────────────┐
│ CPU Socket                                             │
│ ┌────────────────────────────────────────────────────┐ │
│ │ Integrated Memory Controller (Direct to DDR4/DDR5) │ │
│ ├────────────────────────────────────────────────────┤ │
│ │ PCIe Root Complex (High-speed x16 lanes to GPU/NIC)│ │
│ └─────────────────────────┬──────────────────────────┘ │
└───────────────────────────┼────────────────────────────┘
                            │ Direct Media Interface (DMI) Bus
┌───────────────────────────▼────────────────────────────┐
│ Platform Controller Hub (PCH / Southbridge)            │
│ ├── SATA 6 Gbps Controller (SATA SSDs / HDDs)          │
│ ├── USB 3.2 / USB4 Controllers                         │
│ ├── Gigabit Ethernet Physical Layer                    │
│ └── SPI Bus (UEFI Firmware ROM)                        │
└────────────────────────────────────────────────────────┘
```

---

## 2. Expansion Buses: PCIe (Peripheral Component Interconnect Express)

**PCIe** is the universal high-speed point-to-point serial expansion bus in contemporary servers, linking GPUs, high-speed Network Cards (NICs), NVMe storage drives, and AI accelerators directly to the CPU Root Complex.

- **Lanes:** PCIe connections are composed of parallel differential signaling pairs called **lanes** ($x1, x4, x8, x16$). An $x16$ slot provides 16 full-duplex lanes.
- **Generations & Bandwidth Scaling (Per $x16$ Slot):**
  - **PCIe 3.0:** ~15.75 GB/s
  - **PCIe 4.0:** ~31.5 GB/s (Dominant in modern cloud nodes)
  - **PCIe 5.0:** ~63.0 GB/s (Standard for high-bandwidth AI clusters, NVIDIA H100)
  - **PCIe 6.0:** ~126.0 GB/s (PAM4 signaling)

---

## 3. GPU (Graphics Processing Unit) in Modern DevOps & AI

While a CPU core is an ultra-fast, low-latency, general-purpose scalar processor optimized for sequential logic, a **GPU** is a massively parallel throughput architecture consisting of thousands of smaller, energy-efficient ALUs:

```text
CPU Architecture (Latency-Optimized)     GPU Architecture (Throughput-Optimized)
┌───────────────────────────────────┐    ┌───────────────────────────────────┐
│ Large Cache, Complex Control Unit │    │ Tiny Cache, Simple Control Logic  │
├─────────┬─────────┬───────────────┤    ├─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┬─┤
│ Core 0  │ Core 1  │ Core 2 ...    │    │ Thousands of SIMD/Tensor Cores    │
└─────────┴─────────┴───────────────┘    └─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┴─┘
(4 to 128 general-purpose cores)         (4,000 to 18,000+ parallel compute cores)
```

### DevOps & Cloud Role of GPUs
- **AI / LLM Model Training & Inference:** Matrix multiplication acceleration via CUDA and Tensor Cores (NVIDIA A100, H100, L4).
- **GPU Virtualization in Kubernetes:** The **NVIDIA GPU Operator** automates the injection of kernel drivers, container runtime hooks, and monitoring exporters (`dcgm-exporter`), allowing pods to request fractional or dedicated GPU resources via Kubernetes device plugins (`nvidia.com/gpu: 1`).

---

## 4. NIC (Network Interface Controller) & MAC Addresses

The **NIC** bridges the physical server to local area networks (LANs) and WANs:
- **Layer 2 MAC Address (Media Access Control):** A globally unique 48-bit hardware identifier burned into the NIC's EEPROM (e.g., `00:1A:2B:3C:4D:5E`).
  - First 24 bits: **OUI (Organizationally Unique Identifier)** assigned by IEEE to the manufacturer (Intel, Broadcom, Realtek).
  - Last 24 bits: Unique device identifier.
- **Hardware Offloads:** Enterprise NICs feature onboard microprocessors providing hardware offloading for TCP Checksums, Large Send Offload (LSO), and Single Root I/O Virtualization (SR-IOV) for near bare-metal networking speeds inside virtual machines.

---

## 5. Hardware Interrupts & Interrupt Handling

A computer cannot waste CPU cycles constantly polling hardware peripherals to see if a key was pressed, a disk read finished, or a network packet arrived. Instead, it relies on **Interrupts**:

```text
1. NIC receives network packet from Ethernet cable
                        │
                        ▼
2. NIC asserts hardware Interrupt Request line (IRQ) on PCIe bus
                        │
                        ▼
3. Advanced Programmable Interrupt Controller (APIC) halts CPU
                        │
                        ▼
4. CPU pauses user program, saves registers to stack, and loads the
   Interrupt Service Routine (ISR) defined in the Interrupt Descriptor Table (IDT)
                        │
                        ▼
5. Kernel handles packet, schedules softirq, restores registers, resumes user app
```

---

## 6. Direct Memory Access (DMA)

Without **DMA (Direct Memory Access)**, the CPU would be forced to execute a `read` instruction and a `write` instruction for every single 4-byte chunk of data transferred between a disk/NIC and RAM.

DMA is specialized hardware inside peripherals (or chipset controllers) that permits devices to transfer data **directly to and from system RAM without CPU intervention**:
1. The CPU programs the DMA controller with source address, target RAM address, and byte length.
2. The CPU is freed to execute unrelated application threads.
3. The DMA controller transfers the data across the system bus.
4. Upon completion, the DMA controller raises a single hardware interrupt to notify the CPU that the transfer is complete.
