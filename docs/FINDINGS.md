# SGX544 findings — evidence ledger

**Scope:** initial static reverse engineering of `libGLESv2_POWERVR_SGX544_115.so`, an x86 32-bit stripped ELF analyzed in Ghidra with image base `0x0`. All offsets below are **analysis addresses in this specific binary**, not portable hardware registers or addresses in every build.

## Confirmed from current analysis

| ID | Observation | Evidence / location | Limit |
|---|---|---|---|
| F01 | The library exports or contains the `glCompileShader` path. | Entry at `0x3A4D0` | Static call-path observation only. |
| F02 | Shader compilation uses an indirect callback from context offset `+0x5A8`. | Call near `0x3A6CA` | The callback is populated separately. |
| F03 | Initialization attempts to load `libglslcompiler.so`. | Function `FUN_00037C90`; `PVRSRVLoadLibrary` | Loading was traced statically, not demonstrated at runtime. |
| F04 | A symbol named `GLSLCompileToUniflex` is resolved and assigned to `+0x5A8`. | `PVRSRVGetLibFuncAddr` calls in `FUN_00037C90` | Does **not** locate final USSE encoding. |
| F05 | Other compiler-related callbacks occupy offsets `+0x5A0` through `+0x5B8`. | See `COMPILER_INTERFACE.md` | Names and offsets apply to this analyzed binary. |
| F06 | `FUN_0003BE60` sets up UniPatch-related state and two small dummy USSE allocations. | `ctx+0x4B0`; 8-byte blocks with dwords `0x00000000`, `0xFA4C0700` | The exact runtime role of these blocks is not fully established. |
| F07 | `FUN_0006C2F0` allocates/zeros `0x314` bytes and stores callback pointers. | Decompiled allocation path | Purpose of the full object remains uncertain. |
| F08 | `FUN_0006C870` calls a stored free callback. | Cleanup path | Not a GLSL compiler. |
| F09 | `FUN_0006C3C0` consumes the result of the compile callback in a shader-object construction path. | `glCompileShader` downstream path | Exact UniFlex object layout not yet recovered. |

## Known missing material

- `libglslcompiler.so` was **not present in the supplied archive** of Android blobs. Incogninto has been asked to look under `/vendor/lib` and `/system/lib`.
- No SGX544 native hardware tests, GPU execution, or readback have been reported in this investigation.
- No SGX544-specific command-submission protocol or MMU contract has been validated.

## Hypotheses and open questions

- Where does UniFlex become SGX544 USSE machine code: in the GLSL compiler, another component, or a later codegen stage?
- Which components create PDS programs and encode task-control information?
- What are the actual kernel device interfaces, ioctls, memory handles, and synchronization requirements for the device?
- Can existing SGX535-reMESA Gallium code be shared without hiding important hardware differences?

## Important distinction

`GLSLCompileToUniflex` establishes an identified **GLSL → UniFlex interface**. It does **not**, by itself, prove that the final **UniFlex → USSE encoder** has been found or that a standalone shader compiler can be reconstructed from the available binaries.
