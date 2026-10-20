# Source provenance and clean-room policy

## Inputs to the initial investigation

- Archive shared by **Incogninto**: `PowerVR-Android-blobs.tar.gz` (reported size: **903.22 KB**), described by its provider as coming from `/vendor/lib` on a Samsung Android tablet.
- Analyzed library: `libGLESv2_POWERVR_SGX544_115.so` (x86 32-bit stripped ELF; Ghidra image base 0).
- The requested `libglslcompiler.so` was not present in the provided archive.

The exact device model, provenance of each binary, checksums, and driver-version compatibility have **not** yet been independently verified. The archive is **not included** in this repository.

## Rules

1. Do not commit vendor `.so` files, decompiler exports of implementation bodies, or proprietary source code.
2. Record facts about interfaces, observed behavior, offsets, test vectors, and experimental results with their evidence level.
3. Implement independently; avoid copying proprietary expression or binary code.
4. Track each artifact's filename, SHA-256, tool version, and hardware revision privately when available.
5. Distinguish static analysis, proprietary-driver behavior, and **independent driver** behavior.
6. Obtain the device owner's permission before testing any potentially disruptive GPU commands.

## Evidence labels

- **CONFIRMED (STATIC):** directly observed in the analyzed binary.
- **CONFIRMED (HARDWARE):** repeatable result measured on SGX544 hardware.
- **INFERRED:** interpretation consistent with evidence but not directly established.
- **UNKNOWN:** insufficient evidence.

There are currently **no CONFIRMED (HARDWARE)** SGX544 results in this project.
