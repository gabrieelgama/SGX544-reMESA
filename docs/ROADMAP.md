# Roadmap (not a schedule)

- [x] Obtain initial SGX544 Android PowerVR library archive.
- [x] Locate `glCompileShader` and its indirect compiler callback.
- [x] Identify dynamic `libglslcompiler.so` dependency and compiler symbols.
- [x] Record early UniPatch/shader-object paths.
- [ ] Obtain and analyze the matching `libglslcompiler.so` if available.
- [ ] Reconstruct enough UniFlex structure to describe input/output contracts.
- [ ] Identify the SGX544 USSE encoding path and PDS preparation.
- [ ] Identify and validate device/kernel memory and submission contracts.
- [ ] Arrange controlled tests on actual SGX544 hardware.
- [ ] Demonstrate independent GPU execution with verified readback.
- [ ] Build minimal Gallium screen/context/resource support.
- [ ] Implement a restricted shader/draw path and automated regressions.
- [ ] Expand OpenGL ES compatibility, correctness and stability.

**No estimated completion date:** missing binaries and hardware access could change the effort substantially.
