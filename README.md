# SGX544-reMESA

*Making a 2012 PowerVR potato earn its pixels again 🥹*

An experimental, open-source Mesa/Gallium driver project for the **PowerVR SGX544**.

**Special thanks to Incogninto for providing the original Android PowerVR driver binaries that made this project possible!**

I'm the developer behind [SGX535-reMESA](https://github.com/gabrieelgama/SGX535-reMESA), where I've been working on bringing hardware-accelerated 3D graphics back to Intel's infamous Poulsbo chipset.

Now I'm giving the SGX544 a shot too.

## What's this about?

The PowerVR SGX544 was used in quite a few mobile devices back in the day. Unfortunately, like many older PowerVR GPUs, its graphics support relies heavily on proprietary drivers.

I want to see how much of that can be rebuilt using clean-room reverse engineering and Mesa.

I'm currently investigating the original Android graphics libraries, figuring out how they handle shaders, communicate with the GPU, and generate the instructions that eventually run on the hardware.

The goal is to write an independent, open-source driver rather than depend on the original proprietary implementation.

## Current progress

This project is still very early in development.

So far, I've managed to:

- Start reverse engineering the original PowerVR SGX544 OpenGL ES driver.
- Trace the `glCompileShader` path.
- Identify how the driver dynamically loads `libglslcompiler.so`.
- Find the `GLSLCompileToUniflex` compiler interface.
- Identify several compiler initialization, shutdown, and shader management callbacks.
- Begin investigating the relationship between GLSL, UniFlex, and USSE.

There's still a lot to figure out, especially around shader code generation, PDS, GPU memory management, and command submission.

**There is no working open-source SGX544 driver here yet.**

For now, this repository is where I'll keep the research, experiments, and eventually the driver itself.

## The plan

I'd like to get basic GPU rendering working first, then gradually build toward a usable Mesa/Gallium driver.

I'm also hoping to reuse some of the work from SGX535-reMESA, although the SGX544 is a different GPU and I'll need to investigate those differences properly.

Eventually, I'd love to have both projects sharing more code, but I'm keeping them separate for now.

## Hardware testing

I don't currently own an SGX544 device.

The initial research is being done using Android PowerVR driver binaries provided by Incogninto, who owns compatible hardware. I'm hoping to arrange real hardware testing as the project progresses.

Until then, any findings based only on static analysis will be treated as unverified until they can be tested.

## Clean-room reverse engineering

This project is based on independently written code and analysis of existing software and observable GPU behavior.

Proprietary driver binaries and copyrighted implementation code will not be included in this repository.

The goal is to understand the hardware well enough to write our own implementation.

## License

MIT License.

## One more thing

I already spent almost a month figuring out what a single instruction meant on the SGX535

Hopefully this GPU will be a little nicer to me

Probably not
