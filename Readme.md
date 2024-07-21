# Vulkan Basics
This repo follows https://vulkan-tutorial.com/ as a source reference with some modifications of my own to create a 3d renderer. My goal was to learn the basics of vulkan and how it works.
This is in a single file for the most part.

Supports
- Depth Buffer
- Textures (Single per object) using `stb_iamge`
- Model loading using `tiny_obj_loader` library for obj objects
- Automatic runtime mipmap generation
- Multisampling

> Note: Currently the windowing library only supports X11 at the moment on linux unlike in the reference. For a learning project windows wasn't really a priority although it is probably easy to build in :P


# Compilation
compilation should be very easy with cmake installed (ninja route is recommended)

- Create the build directory
```
mkdir build && cd build
```

- Generate Make file or build.ninja with `COMPILE_SHADER` option turned true
```
cmake .. -DCOMPILE_SHADER=true

// or

cmake .. -GNinja -DCOMPILE_SHADER=true
```

- Build
```
make .

// or

ninja -j 2
```
