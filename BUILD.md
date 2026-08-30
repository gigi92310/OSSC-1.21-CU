# OSSC Classic 1.21-cu4 — primary build

## Target

This repository documents and builds **Primary / FW1 only**.

- Firmware identity: `OSSC Classic 1.21-cu4`
- Updater key: `OSSC`
- FPGA / Ibex boot address: `0x02050000`
- Software flash/link origin: `0x02050000`
- Firmware suffix: `cu4`

No secondary/FW2 image or secondary build-address patch is part of this repository.

## Upstream base

Official OSSC v1.21 source repository: `marqs85/ossc`

Exact upstream commit used as the reference base:

`eee3950f2ee4a9b3d31ba4f0986a99500a7c216f`

## Software target

The software target is RV32EMC with ABI `ilp32e`.

The verified CU4 software image was built with:

- Clang/LLD 17
- optimization: `-Oz`
- no LTO

Verified software entry point: `0x020500b4`

Verified software flash payload size: `77056` bytes

## FPGA image

CU4 is a software-only functional update relative to the validated CU3 primary build. The primary CU4 image therefore reuses the validated CU3 primary FPGA RBF. The custom software remains linked for the primary flash origin at `0x02050000`.

## Reconstructing the source

`source/` contains the complete custom source delta split into 10 numbered per-file patches.

From the root of the exact upstream v1.21 commit, apply them in numeric order:

```sh
for p in source/[0-9][0-9]-*.patch; do
    patch -p1 < "$p"
done
```

Then initialize/use the official submodule revisions and build environment and build the **primary/FW1** target.

`patches/ossc-v1.21-cu4.patch` is retained as the smaller CU3 → CU4 development/reference delta; the numbered `source/` patch set is the reconstruction set to use from stock v1.21.

## Expected final firmware image

The binary is intentionally not committed here. When added manually, use:

`firmware/ossc_1.21-cu4.bin`

Expected properties:

- updater key: `OSSC`
- primary target
- size: `405504` bytes
- exactly `99 × 4096` byte clusters
- 512-byte aligned
- SHA-256: `01108dfef1c1416c47a970cc09548e3afd116473d7fe4cf460f2dcd44d6a7a8e`

## Verification performed

The validated CU4 primary image passed checks for firmware header key, version/suffix, header CRC, firmware data CRC, embedded primary RBF, bit-swapped software recovery, software entry point, final `0xFF` padding, total image size and updater cluster limit.

The input classifier was checked with representative modes for 240p, 288p, Atari ST HIGH, 400p/70, 480i, 576i, 480p, 576p, 720p, 1080i and 1080p, including override fallback to base mappings.
