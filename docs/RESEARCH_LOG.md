# Research log

## 2026-10-16

Incogninto offered to provide PowerVR Android graphics binaries from a Samsung tablet.

## 2026-10-18

Incogninto shared `PowerVR-Android-blobs.tar.gz` (903.22 KB), described as originating from `/vendor/lib`.

## 2026-10-20 — initial static analysis

- Inspected `libGLESv2_POWERVR_SGX544_115.so` in Ghidra (x86 32-bit, stripped, image base zero).
- Located `glCompileShader` at `0x3A4D0` and an indirect callback near `0x3A6CA` using `ctx+0x5A8`.
- Traced initialization in `FUN_00037C90`: load `libglslcompiler.so` and resolve compiler/SGXBS symbols.
- Documented callbacks at context offsets `+0x5A0`–`+0x5B8`.
- Investigated UniPatch setup and several allocation/cleanup functions.
- Asked Incogninto to look for the missing `libglslcompiler.so` in `/vendor/lib` or `/system/lib`.
- Asked about root and ADB access for potential future hardware testing.

**Status:** static findings only; no independent SGX544 rendering.
