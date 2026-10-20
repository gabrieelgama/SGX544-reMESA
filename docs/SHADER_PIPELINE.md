# Shader pipeline — known edges and unknown edges

```text
Application GLSL source
        |
        v
OpenGL ES glCompileShader (0x3A4D0)
        |
        v
Context callback +0x5A8
        |
        v
GLSLCompileToUniflex  [symbol resolved, implementation unavailable]
        |
        v
UniFlex output         [format not recovered]
        |
        +----> FUN_0006C3C0 / shader object path [observed consumer]
        |
        v
[Unknown backend lowering / USSE encoding]
        |
        v
[Unknown PDS / task-control preparation]
        |
        v
[Unknown kernel submission + GPU execution]
```

**Important:** This diagram is a map of research questions, **not** a verified complete execution pipeline. The ordering of the unknown backend and PDS stages is illustrative and must be established from evidence.

## Research tasks

- Locate `libglslcompiler.so` for the exact driver version, if legally shareable for local analysis.
- Inspect `GLSLCompileToUniflex` arguments, output objects, and error reporting.
- Determine whether `SGXBS_CreateBinaryShader` and `SGXBS_CreateBinaryProgram` serialize or compile shader programs.
- Trace data from compiled shader objects into hardware memory allocations and command buffers.
- Compare SGX544 observations with SGX535-reMESA **only where verified compatible**.
