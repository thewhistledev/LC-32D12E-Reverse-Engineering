# Sharp LC-32D12E Reverse Engineering

> **Firmware archaeology, hardware analysis and boot-process research for the Sharp LC-32D12E television platform.**

![Status](https://img.shields.io/badge/status-active%20research-red)
![Platform](https://img.shields.io/badge/platform-Sharp%20LC--32D12E-black)
![Firmware](https://img.shields.io/badge/firmware-tv.bin-blue)
![Bootloader](https://img.shields.io/badge/bootloader-U--Boot-orange)

This repository is a working reverse-engineering archive for the **Sharp LC-32D12E** TV. It records the hardware investigation, firmware extraction, binary analysis, disassembly and ongoing reconstruction of the platform's boot process and memory layout.

The aim is simple: preserve the evidence, document what can actually be proven, and gradually turn an undocumented consumer-TV firmware image into something other researchers can inspect and reproduce.

> **Research status:** Active and incomplete. Confirmed findings, strong indications and hypotheses are deliberately kept separate.

---

## Contents

- [Target](#target)
- [Current findings](#current-findings)
- [Hardware](#hardware)
- [Firmware](#firmware)
- [Reverse engineering](#reverse-engineering)
- [Repository layout](#repository-layout)
- [Evidence policy](#evidence-policy)
- [Roadmap](#roadmap)
- [Contributing](#contributing)
- [Disclaimer](#disclaimer)

---

## Target

| Property | Finding |
|---|---|
| Manufacturer | **Sharp** |
| Model | **LC-32D12E** |
| Platform | Sharp / Vestel-derived hardware under investigation |
| Relevant memory | **U10** |
| U10 marking | `E1116AE8G` |
| Firmware dump | `tv.bin` |
| Bootloader evidence | **U-Boot strongly indicated** |
| CPU family | **MIPS-family evidence present** |
| Exact CPU / SoC | **Not yet confirmed** |
| ISA mode | **Not yet confirmed** |
| Memory map | **Under reconstruction** |
| Debug interface | **Not yet confirmed** |

---

## Current findings

### Confirmed / directly observed

- The target hardware is a **Sharp LC-32D12E** television.
- The investigated non-volatile memory device is located at **U10** and is marked `E1116AE8G`.
- A firmware dump named **`tv.bin`** is available for analysis.
- The dump contains bootloader and broader firmware material rather than being merely a small configuration/data dump.
- Analysis produced bootloader-related extracted artifacts, including U-Boot-related material and assembly output.
- The resulting disassembly contains substantial **MIPS-family instruction evidence**, including CP0 operations and conventional MIPS register usage.

### Strongly indicated

- **U-Boot** is present or represented in the extracted firmware material.
- The firmware is executing in a MIPS-family environment.

### Still unresolved

- Exact main SoC / processor part number.
- Exact ISA revision and execution mode.
- Standard MIPS32 versus microMIPS for the relevant regions.
- Firmware reset vector and true entry point.
- Physical-to-virtual memory mapping.
- RAM base and size.
- Peripheral register map.
- Complete flash partition layout.
- Exact U-Boot version/build.
- Kernel and root filesystem boundaries.
- UART/JTAG/debug interface location.

---

## Hardware

### U10

The component identified during board inspection is:

```text
U10
E1116AE8G
```

U10 is the relevant non-volatile memory device investigated during the firmware-dump work.

It is important to distinguish **hardware from software** here. U10 should not simply be called “the bootloader chip”. The bootloader is software stored in non-volatile memory. U10 is the physical memory device being investigated as the storage location for that software and associated firmware data.

The exact manufacturer, device identity and complete geometry still require authoritative confirmation.

### Debug interfaces

UART, JTAG and board test points remain under investigation. A test point will not be documented as UART merely because it looks plausible. Confirmation requires electrical or serial evidence.

---

## Firmware

The main firmware image currently under investigation is:

```text
tv.bin
```

Initial extraction and reverse engineering produced the following derived artifacts:

```text
boot_stub.bin
boot_stub2.bin
boot_stub3.bin
uboot_payload.bin
uboot.asm
uboot(1).asm
tv.asm
asl.asm
strings.txt
interesting.txt
```

The relationships between these regions are still being reconstructed. The repository therefore treats these as **analysis artifacts**, not automatically as independent firmware partitions.

See [`FINDINGS.md`](FINDINGS.md) and [`artifacts/ARTIFACT_STATUS.md`](artifacts/ARTIFACT_STATUS.md) for the current artifact inventory.

---

## Reverse engineering

### MIPS-family evidence

The generated assembly contains instructions and register conventions strongly associated with MIPS-family processors, including examples such as:

```asm
mfc0
mtc0
jr $ra
jalr $t9
lui
addiu
lw
sw
```

There are also CP0 register accesses and conventional MIPS calling/return patterns.

This is useful architectural evidence, but it does **not** by itself establish the exact processor or prove that every region uses the same instruction mode.

### microMIPS investigation

microMIPS has been considered because of the firmware's instruction decoding and the unusual mixture of decoded instructions and raw byte sequences in some regions.

At present:

> **microMIPS is a hypothesis, not a confirmed architectural identification.**

Further validation should use reset-vector behaviour, known code/data boundaries, instruction alignment, SoC identification and cross-checking against the physical platform.

### U-Boot

The extracted firmware contains substantial code consistent with a U-Boot-derived bootloader environment. The repository records this as a strong finding while avoiding an unsupported claim about the exact U-Boot release or vendor modifications.

Future work should identify:

- U-Boot version/build strings
- command tables
- environment storage
- board initialisation
- memory initialisation
- flash drivers
- boot arguments
- kernel hand-off
- recovery/update paths

---

## Repository layout

```text
.
├── README.md
├── FINDINGS.md
├── artifacts/
│   └── ARTIFACT_STATUS.md
└── analysis/
    └── future analysis scripts and derived material
```

The repository will grow as more original material can be safely preserved.

### Planned structure

```text
artifacts/
├── original/       # untouched source dumps
├── extracted/      # extraction results
└── hashes/         # SHA-256 and other integrity records

analysis/
├── binwalk/        # extraction reports
├── disassembly/    # assembly and architecture experiments
├── strings/        # string/signature analysis
└── scripts/        # reproducible analysis tooling

docs/
├── hardware/      # PCB and component documentation
├── firmware/      # firmware format and memory-map notes
└── interfaces/    # UART/JTAG/test-point research
```

---

## Evidence policy

This project deliberately separates three levels of confidence.

### Confirmed

Directly observed from the hardware, original dump or reproducible analysis.

### Strong indication

Supported by multiple pieces of evidence but still worth independently validating.

### Hypothesis

A plausible interpretation requiring further evidence.

This distinction matters particularly for CPU identification, memory maps and firmware partition boundaries. A disassembler producing sensible-looking instructions is evidence, not divine revelation.

---

## Preservation policy

Original firmware dumps are treated as **read-only evidence**.

Before any modification or programming operation:

1. Preserve the complete original dump.
2. Calculate a SHA-256 hash.
3. Keep the original separately from derived files.
4. Perform experiments on copies.
5. Record transformations and offsets.
6. Never replace the original evidence with a modified image.

Where possible, every released binary artifact should eventually have an integrity record.

---

## Roadmap

### Hardware

- [ ] Confirm exact U10 memory device
- [ ] Identify main SoC
- [ ] Record board revision and markings
- [ ] Map significant test points
- [ ] Identify UART
- [ ] Investigate JTAG / debug access
- [ ] Document power and boot-related circuitry

### Firmware

- [x] Preserve initial `tv.bin` analysis
- [x] Extract bootloader-related material
- [x] Generate assembly output
- [x] Identify strong MIPS-family evidence
- [ ] Confirm reset vector
- [ ] Confirm CPU / SoC
- [ ] Confirm MIPS32 vs microMIPS
- [ ] Reconstruct memory map
- [ ] Reconstruct flash partitions
- [ ] Identify compression/encryption
- [ ] Identify U-Boot version/build
- [ ] Locate kernel
- [ ] Locate root filesystem
- [ ] Identify update/recovery mechanism

### Tooling

- [ ] Reproducible extraction script
- [ ] Automated firmware signature scanner
- [ ] Hash-manifest generator
- [ ] Firmware region visualiser
- [ ] Ghidra loading notes
- [ ] MIPS/microMIPS validation helpers
- [ ] Automated string/reference analysis

### Documentation

- [x] Initial findings
- [x] Artifact inventory
- [ ] Complete board documentation
- [ ] Firmware format specification
- [ ] Memory map
- [ ] Partition table
- [ ] Debug interface documentation
- [ ] Reproduction guide

---

## Contributing

Useful contributions should be backed by reproducible evidence.

For binary-analysis findings, include where possible:

- filename
- offset
- size
- extraction method
- architecture/mode used
- relevant command or tool
- interpretation
- confidence level

For hardware findings, include:

- component reference
- exact marking
- board location
- photograph where appropriate
- measurement or serial evidence where available

Please avoid silently turning a hypothesis into a confirmed fact.

---

## Disclaimer

This repository is intended for firmware research, preservation, interoperability, education and security research.

Do not write experimental firmware to hardware without preserving a complete recovery path and original dump first. Hardware modification can permanently damage equipment or leave it unbootable.

---

## Project status

**Active reverse engineering / incomplete**

The LC-32D12E firmware has **not** been completely reverse engineered. This repository is a living research archive and findings may be corrected when stronger evidence becomes available.

---

## Documentation

- [`FINDINGS.md`](FINDINGS.md) — consolidated hardware and firmware findings
- [`artifacts/ARTIFACT_STATUS.md`](artifacts/ARTIFACT_STATUS.md) — analysis artifact inventory and preservation status

---

**Target:** Sharp LC-32D12E  
**Repository:** `thewhistledev/LC-32D12E-Reverse-Engineering`  
**Branch:** `main`  
**Status:** Active research
