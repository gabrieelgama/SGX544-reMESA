# Hardware testing plan

**Status:** No SGX544 hardware execution by this project has been confirmed.

The developer does not own an SGX544 device. Incogninto provided Android graphics blobs and may be able to help with tests later; root, ADB access, permissions, and participation have **not** been confirmed.

## First questions for the device owner

- Exact tablet model, Android version, SoC and PowerVR core variant.
- Is ADB available? Is the device rooted?
- Which graphics/kernel devices exist, and what are their permissions?
- Can a native test binary run safely and produce logs?
- Can the owner restore the device if a graphics experiment hangs the UI?

## Read-only environment inventory

Run on a trusted host with ADB installed:

```sh
adb devices
adb shell id
adb shell getprop ro.product.model
adb shell getprop ro.build.version.release
adb shell ls -l /dev/pvrsrvkm* /dev/dri/* 2>/dev/null
adb shell su -c id
```

`su` may fail on an unrooted device; that is an expected result. Device nodes vary by platform and Android version. Do not publish serial numbers, personal data, or full device logs without review.

## Progressive test gates

1. **Inventory:** collect device identity, driver/kernel versions, permissions.
2. **Offline:** validate parsers, buffer layouts, and expected outputs without touching the GPU.
3. **Read-only inspection:** compare observed driver interfaces and memory formats.
4. **Controlled submission:** attempt minimal GPU operations only with the owner's informed consent, a recovery path, and an explicit safety gate.
5. **Readback:** verify exact framebuffer contents and completion signals; distinguish CPU-written pixels from GPU output.
6. **Repeatability:** multiple clean runs, no UI freezes, no unexplained memory corruption.
7. **Gallium integration:** basic resources, draws, shaders, synchronization and later conformance tests.

Do not infer shader execution from `glClear` alone: clear operations may use a different path. A controlled draw with a known shader and validated readback is stronger evidence.

## Suggested test report fields

```text
Device / SoC:
GPU variant / revision:
Android / kernel:
Test build hash:
Root / ADB availability:
Device node / interface:
Test case:
Submission status:
Completion / retirement evidence:
Framebuffer readback and expected hash:
UI / system stability:
Logs / artifacts:
Result: PASS | FAIL | INCONCLUSIVE
```
