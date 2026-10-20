# Function map (current binary only)

**Library:** `libGLESv2_POWERVR_SGX544_115.so`  
**Architecture:** x86 / ELF32 / stripped  
**Ghidra image base:** `0x00000000`

| Function | Observed behavior | Confidence |
|---|---|---|
| `glCompileShader` @ `0x3A4D0` | Compilation entry path, indirect call via `ctx+0x5A8` near `0x3A6CA` | High for call path |
| `FUN_00037C90` | Loads `libglslcompiler.so`, resolves compiler and SGXBS callbacks | High for symbol-resolution path |
| `FUN_0003BE60` (`CreateProgramState` in analysis) | Initializes UniPatch state at `ctx+0x4B0`; allocates two 8-byte dummy USSE blocks | High for operations, lower for intended purpose |
| `FUN_0006C2F0` | Allocates and zeroes `0x314` bytes; records malloc/free and optional callback | High for operations |
| `FUN_00036130` | Eight NOPs followed by RET; appears as an optional callback | High for instructions, role unknown |
| `FUN_0003C2A0` | Wrapper calling `FUN_0003C050` | High |
| `FUN_0003C050` | Releases shader-associated resources | Medium; object semantics incomplete |
| `FUN_0006C870` | Invokes a stored free callback | High |
| `FUN_0006C3C0` | Handles compilation result in shader-object path | Medium; data layout unknown |

## UniPatch observation

Two allocated blocks were observed with the following 32-bit words:

```text
0x00000000
0xFA4C0700
```

Do **not** assume this pair is a complete executable shader or a portable USSE program without further evidence.

## Next work

1. Cross-reference each function with x86 assembly, not just decompiled C.
2. Recover argument types and object layout for `GLSLCompileToUniflex`.
3. Track ownership/lifetime of compiler output passed into `FUN_0006C3C0`.
4. Resolve how UniPatch and SGXBS interfaces relate to hardware upload.
