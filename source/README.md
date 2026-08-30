# CU4 primary custom source

This directory contains the complete custom source delta for **OSSC Classic 1.21-cu4**, **Primary / FW1 only**.

Upstream repository: `marqs85/ossc`

Exact official v1.21 reference commit:

`eee3950f2ee4a9b3d31ba4f0986a99500a7c216f`

Primary target:

- FPGA / Ibex boot address: `0x02050000`
- software flash/link origin: `0x02050000`
- firmware suffix: `cu4`

No secondary/FW2 target changes are included.

## Modified upstream files

The numbered patches cover these 10 upstream files:

1. `ossc.qsf`
2. `software/sys_controller/av_controller.c`
3. `software/sys_controller/ic_drivers/it6613/typedef.h`
4. `software/sys_controller/inc/av_controller.h`
5. `software/sys_controller/inc/avconfig.h`
6. `software/sys_controller/inc/firmware.h`
7. `software/sys_controller/inc/userdata.h`
8. `software/sys_controller/src/firmware.c`
9. `software/sys_controller/src/menu.c`
10. `software/sys_controller/src/userdata.c`

## Reconstructing the CU4 primary source

Start from the exact upstream commit above, then apply every patch in this directory **in numeric order** from the root of the OSSC source tree:

```sh
for p in source/[0-9][0-9]-*.patch; do
    patch -p1 < "$p"
done
```

The per-file patches contain both the earlier custom additions and the CU4 refinements where required, so the resulting tree is the CU4 primary source state.

Expanded third-party submodules are deliberately not duplicated here. Initialize/use the submodule revisions referenced by the official OSSC v1.21 repository/build environment.

`../patches/ossc-v1.21-cu4.patch` is retained separately as the smaller **CU3 → CU4** development/reference delta.
