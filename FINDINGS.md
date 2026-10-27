# Sharp LC-32D12E Reverse Engineering Findings

> Status: ongoing research. This document records observations from physical-board inspection and firmware analysis. Conclusions marked as uncertain should not be treated as confirmed hardware specifications.

## Target

- **TV:** Sharp LC-32D12E
- **Project:** Reverse engineering the factory firmware and hardware platform.

## Hardware observations

### U10

The board component identified as **U10** is marked:

`E1116AE8G`

U10 has been identified as the relevant non-volatile memory device investigated during the firmware work. The board inspection did not identify another obvious SPI NOR device that clearly corresponded to the main TV firmware.

The exact manufacturer/device interpretation of the `E1116AE8G` marking and its complete memory organisation still requires confirmation from a datasheet or authoritative hardware identification.

## Firmware image

A firmware dump named **`tv.bin`** has been analysed.

Current evidence indicates that `tv.bin` contains the TV's bootloader/firmware payload rather than being a simple user-data or configuration dump.

### Analysis performed

`binwalk` was used against the image. The extraction identified data at least consistent with an embedded bootloader payload. During analysis the following working artifacts were produced:

- `boot_stub.bin`
- `boot_stub2.bin`
- `boot_stub3.bin`
- `uboot_payload.bin`
- `uboot.asm`
- `uboot(1).asm`
- `tv.asm`
- `asl.asm`
- string dumps / interesting-data reports

These artifacts are analysis outputs and should not automatically be assumed to represent complete executable images.

## U-Boot

The firmware analysis strongly indicates the presence of **U-Boot** or a U-Boot-derived bootloader component.

A separated payload was identified during extraction and examined as assembly. Further work is required to establish the exact CPU architecture and instruction encoding with confidence.

### CPU architecture

The processor architecture has **not been conclusively established** from the evidence recorded so far. MIPS/microMIPS has been considered during disassembly, but this should remain an investigation hypothesis rather than a confirmed specification until instruction decoding, reset vectors, register conventions, or board-level processor identification provide corroboration.

## What is currently known

1. The target TV is a Sharp **LC-32D12E**.
2. **U10** is marked `E1116AE8G`.
3. `tv.bin` is a firmware image containing bootloader/firmware material.
4. Firmware analysis produced material associated with a U-Boot payload.
5. The exact CPU architecture remains to be confirmed.
6. The exact mapping between the extracted firmware regions and physical memory layout remains under investigation.

## Next investigation targets

- Identify `E1116AE8G` conclusively and document its memory geometry.
- Determine the CPU from board markings, firmware reset vectors, and verified instruction decoding.
- Map the `tv.bin` address space to the physical memory layout.
- Identify U-Boot header/environment structures and partition boundaries.
- Determine whether the image contains a compressed filesystem and identify its filesystem format.
- Compare strings, symbols, and code regions against known Sharp/Vestel platform firmware where legally and technically appropriate.
- Document UART/JTAG/test-point findings if a reliable interface is located.

## Safety / preservation

Firmware modifications should be performed on copies of the original dump. Keep an untouched backup of every read from the original hardware before attempting writes or reprogramming.
