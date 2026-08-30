# OSSC Classic 1.21-cu4

Custom OSSC Classic firmware based on the official **v1.21** release.

This repository intentionally publishes **only the primary / FW1 firmware target**.

The firmware binary itself is **not committed here**; it is intended to be added manually as `firmware/ossc_1.21-cu4.bin`.

## Files

- `source/` — complete stock-v1.21 → CU4-primary custom source delta, split into per-file patches.
- `patches/ossc-v1.21-cu4.patch` — CU3 → CU4 development/reference delta.
- `FEATURES.md` — complete description of all additional custom functions.
- `BUILD.md` — primary build target and firmware verification information.
- `VERSION` — firmware identity and primary flash addresses.

## Version

`OSSC Classic 1.21-cu4`

Primary/FW1 updater key: `OSSC`

Primary FPGA/Ibex boot and software flash origin: `0x02050000`

## Main additions over stock OSSC 1.21

The custom branch adds a profile manager (erase/copy/full backup/full restore), automatic profile loading, refined CU4 PAL-mode classification, dedicated Atari ST HIGH detection, mode-change debounce, an enhanced INFO screen, and safer SD-card reads during firmware updates.

See **FEATURES.md** for the complete description.

## Firmware image verification

Expected manual firmware file:

`firmware/ossc_1.21-cu4.bin`

SHA-256:

`01108dfef1c1416c47a970cc09548e3afd116473d7fe4cf460f2dcd44d6a7a8e`

Size: `405504` bytes — exactly `99 × 4 KiB` clusters and 512-byte aligned.

## Upstream

Based on the GPLv3 OSSC project by Markus Hiienkari / marqs85, official v1.21 source.

Exact upstream reference commit:

`eee3950f2ee4a9b3d31ba4f0986a99500a7c216f`
