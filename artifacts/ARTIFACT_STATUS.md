# Analysis Artifacts

This directory is intended to hold extracted firmware-analysis artifacts from the Sharp LC-32D12E investigation.

## Artifacts identified during analysis

- `tv.bin` — original firmware dump under analysis.
- `boot_stub.bin` — extracted boot stub.
- `boot_stub2.bin` — alternate/secondary extracted boot stub.
- `boot_stub3.bin` — alternate/tertiary extracted boot stub.
- `uboot_payload.bin` — extracted U-Boot-related payload.
- `uboot.asm` — disassembly of the U-Boot-related image.
- `uboot(1).asm` — second U-Boot disassembly generated during analysis.
- `tv.asm` — disassembly of the wider `tv.bin` image.
- `asl.asm` — additional disassembly output.
- `strings.txt` — extracted strings from the firmware.
- `interesting.txt` — selected findings/interesting offsets from analysis.

## Current repository state

The analysis notes and conclusions are published in `FINDINGS.md`.

The raw binary artifacts listed above are deliberately not reconstructed or substituted with partial data. The current file interface exposes the saved disassembly text for `tv.asm`, `uboot.asm`, and `uboot(1).asm`, but does not currently provide authorized raw-byte materialization for those stored files. Likewise, the original binary dumps are not currently available as raw uploadable files in this session.

This avoids putting incomplete or fabricated artifacts into the repository. When the original raw files are available again, they should be added unchanged, preferably under this directory, with hashes recorded in a manifest.

## Integrity rule

Keep original dumps untouched. Any modified, unpacked, patched, or regenerated files should use a separate filename and be clearly labelled as derived data.
