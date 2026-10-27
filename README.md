# Sharp LC-32D12E Reverse Engineering

Reverse engineering research into the **Sharp LC-32D12E** TV PAL platform, including its hardware, firmware dump, bootloader and memory layout.

## Current findings

- Target model: **Sharp LC-32D12E**
- Relevant board memory component: **U10**, marked `E1116AE8G`
- Firmware dump under investigation: **`tv.bin`**
- `tv.bin` contains bootloader/firmware material.
- Analysis has identified material associated with **U-Boot**.
- CPU architecture is **not yet conclusively identified**. MIPS/microMIPS has been investigated, but remains unconfirmed.
- The physical memory map and exact partition boundaries are still being reconstructed.

## Documentation

See [`FINDINGS.md`](FINDINGS.md) for the current hardware and firmware analysis record, including confirmed observations, unresolved questions and next investigation targets.

## Status

**Active reverse-engineering project.** Findings are updated as hardware inspection and firmware analysis progress.

Original firmware dumps should always be preserved unchanged before any modification or reprogramming work.
