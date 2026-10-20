# Compiler interface recovered from the GLES binary

Analyzed library: `libGLESv2_POWERVR_SGX544_115.so` (x86 32-bit stripped ELF). Ghidra image base `0x0`.

`FUN_00037C90` attempts to load `libglslcompiler.so` using `PVRSRVLoadLibrary`, then resolves function names using `PVRSRVGetLibFuncAddr`. The resolved pointers are stored in the GLES context.

| Context offset | Resolved symbol | Interpretation |
|---|---|---|
| `+0x5A0` | `GLSLInitCompiler` | Compiler initialization |
| `+0x5A4` | `GLSLShutDownCompiler` | Compiler shutdown |
| `+0x5A8` | `GLSLCompileToUniflex` | Shader compile callback |
| `+0x5AC` | `GLSLDisplayMetrics` | Compiler metrics |
| `+0x5B0` | `GLSLFreeCompiledUniflexProgram` | Release compiled output |
| `+0x5B4` | `SGXBS_CreateBinaryProgram` | Binary program creation |
| `+0x5B8` | `SGXBS_CreateBinaryShader` | Binary shader creation |

## Relevant call chain

```text
glCompileShader                 @ 0x3A4D0
  -> indirect call ctx+0x5A8   ~ 0x3A6CA
       resolved name: GLSLCompileToUniflex
  -> FUN_0006C3C0             consumes compilation output
```

## What this does not tell us

- Whether `libglslcompiler.so` contains the entire backend code generator.
- The binary layout of UniFlex intermediate objects.
- The machine encoding of SGX544 USSE instructions.
- How shader binaries are uploaded or scheduled on the GPU.

## Ghidra caution

Decompilation of PIC/relocated x86 code in `FUN_00037C90` produced expressions resembling:

```c
*(int *)("glVertexAttrib1fv" + param_1 + 0x11)
*(uint *)("glVertexAttrib4fv" + param_1 + 0xb)
```

These are **not trustworthy semantic interpretations** of the actual program. Check the x86 disassembly, relocation records, and address calculations before assigning meaning to these expressions.
