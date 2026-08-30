# OSSC Classic 1.21-cu4 — additional features

This document describes the complete set of custom functions added on top of the official OSSC Classic v1.21 firmware. The repository targets **Primary / FW1 only**.

## 1. Profile manager

### Erase profile

A selected internal profile slot can be erased directly. Slots `0..14` are supported. The `INIT_CONFIG` slot is protected and cannot be erased through this command.

Erasing removes the userdata signature from the selected 64 KiB sector, so the slot is reported as `<empty>` afterwards.

### Copy profile

An internal profile can be copied directly from a selected source slot to a selected destination slot. The source is validated before the destination is erased.

### Backup all profiles and configuration to SD

All 16 userdata sectors are backed up to `/ossc_bak.bin`:

- profiles `0..14`;
- `INIT_CONFIG` / main configuration.

The backup contains a header, length and CRC.

### Restore all profiles and configuration

Restore validates the complete backup, including its CRC, **before any flash sector is erased**. A corrupt or incomplete backup therefore does not destroy the current profile/configuration flash contents.

## 2. Automatic profile selection

Automatic profile loading is optional and disabled by default.

The original custom implementation provided mappings for:

- 240/288p;
- 384/400p;
- 480/576i;
- 480/576p;
- 720p;
- 1080i;
- 1080p.

CU4 refines this into distinct detected classes:

- 240p;
- 288p;
- 384/400p;
- Atari ST HIGH;
- 480i;
- 576i;
- 480p;
- 576p;
- 720p;
- 1080i;
- 1080p.

The existing/base mappings are retained for compatibility. CU4 adds optional override mappings for 288p, 576i, 576p and Atari ST HIGH.

When an override is set to `Base mapping`, the corresponding legacy/base mapping is inherited. This keeps existing 1.21 custom configurations compatible.

## 3. Dedicated Atari ST HIGH detection

CU4 includes a dedicated classifier for Atari ST HIGH timing, typically around **501 lines / 71 Hz** as measured by OSSC.

The detection window is deliberately narrow to avoid classifying ordinary 480p computer timings as Atari ST HIGH.

A dedicated optional Atari ST HIGH profile override can therefore be loaded automatically.

Atari ST LOW and MEDIUM are not split into separate classes because their sync geometry does not reliably identify their pixel-clock resolution. They remain in the appropriate low-resolution progressive class.

## 4. Input mode-change debounce

Initial synchronization remains immediate.

After initial lock, a detected mode change must remain coherent for **3 consecutive status polls**, approximately **30 ms**, before `MODE_CHANGE` is accepted. This reduces false mode-change events from short measurement fluctuations.

## 5. Enhanced INFO screen

The INFO display includes additional diagnostics:

- input total line count;
- progressive/interlaced state;
- raw input clock count;
- input vertical frequency;
- input horizontal frequency;
- input HSYNC width;
- existing output timing information;
- output pixel clock;
- current internal profile;
- firmware version;
- firmware build date.

CU4 also adds:

- `Auto class` — the currently detected automatic-profile class;
- `Auto target` — the profile selected by that class, including disabled/no-mapping states.

## 6. Safer SD-card reads during firmware update

Firmware update handling was hardened against SD-card read failures.

Before destructive flash erase begins, failed firmware data reads are retried up to **3 times**. If reading still fails, the update aborts before flash erase.

Once erase/write has begun, a failed sector read is retried and stale `tmpbuf` data is never written to flash.

## 7. Firmware identity

The custom firmware identifies itself as:

`OSSC Classic 1.21-cu4`

## 8. IT6613 build compatibility cleanup

The IT6613 `DWORD` typedef was changed from `unsigned long` to `unsigned int`. Both are 32-bit on the RV32 target; this removes a host/toolchain compatibility ambiguity without changing the intended target width.

## 9. Classifier validation

Representative classifier tests were checked for NTSC 240p, PAL 288p, Atari ST HIGH, 400p/70, 480i, 576i, 480p, 576p, 720p, 1080i and 1080p, including override fallback to base mapping.

## 10. Primary firmware constraints retained

The stock updater protection around `cluster_idx[100]` is retained. The validated primary CU4 image occupies exactly **99 × 4 KiB clusters**, staying within the effective updater image limit.
