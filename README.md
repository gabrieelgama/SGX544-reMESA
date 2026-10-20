# SGX544-reMESA

*Making a 2012 PowerVR potato earn its pixels again 🥹*

An experimental, open-source Mesa/Gallium driver project for the **PowerVR SGX544**.

**Special thanks to Incogninto** for sharing the Android PowerVR driver binaries that made this investigation possible.

I'm also working on [SGX535-reMESA](https://github.com/gabrieelgama/SGX535-reMESA), which started as an attempt to bring hardware-accelerated 3D graphics back to Intel's Poulsbo chipset. Now I'm giving the SGX544 a shot too.

## Where things stand

This is **early reverse-engineering research**, not a working SGX544 driver. No independent GPU command submission, framebuffer rendering, or Mesa conformance has been demonstrated on SGX544 yet.

From analysis of the original 32-bit x86 PowerVR OpenGL ES library, I've found:

- The `glCompileShader` entry point and its indirect compiler callback.
- Dynamic loading of `libglslcompiler.so` through PowerVR's library loader.
- The `GLSLCompileToUniflex` compiler entry point and related lifecycle callbacks.
- UniPatch initialization and some shader-object cleanup/allocation paths.

That's a useful starting point, but it doesn't mean we've found the final USSE machine-code encoder. See [the findings](docs/FINDINGS.md) for what is confirmed, inferred, and still unknown.

## What I'm trying to do

Build an independent Mesa/Gallium driver, learn how SGX544 programs are compiled and submitted, and eventually test real rendering on hardware. Some infrastructure from SGX535-reMESA may be reusable, but SGX544 differences need their own verification.

I don't own an SGX544 device. The initial binaries came from Incogninto; hardware testing depends on future access and coordination.

## Research notes

- [Findings and evidence levels](docs/FINDINGS.md)
- [Compiler callbacks and offsets](docs/COMPILER_INTERFACE.md)
- [Function map](docs/FUNCTION_MAP.md)
- [Shader pipeline and unknowns](docs/SHADER_PIPELINE.md)
- [Hardware testing plan](docs/HARDWARE_TESTING.md)
- [Roadmap](docs/ROADMAP.md)
- [Provenance and clean-room rules](docs/PROVENANCE.md)
- [Research log](docs/RESEARCH_LOG.md)

## Clean-room policy

No proprietary PowerVR libraries, disassembled implementation bodies, or copied driver code are distributed here. This repository is for independently written code, tests, and documentation of observed behavior. Symbol names and offsets in the research notes are analysis references, not claims of public API stability.

## License

MIT. See [LICENSE](LICENSE).

## One more thing

I already spent almost a month figuring out what a single instruction meant on the SGX535.

Hopefully this GPU will be a little nicer to me.

Probably not. 💀
