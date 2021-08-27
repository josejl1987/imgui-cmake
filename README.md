# imgui-cmake

Simple project with cmake files to build imgui's example_sdl_opengl3 on multiple platforms. This should work on:
- Windows (tested, working)
- Linux (tested, working)
- Mac OS
- Raspberry Pi (tested on Pi3, working)

# How to Build

To test the example:
```
git clone https://github.com/Pesc0/imgui-cmake --recurse-submodules
git submodule update --remote
cd imgui-cmake
mkdir build && cd build
cmake -D BUILD_EXAMPLES=ON ..
cmake --build . -j4
./examples/example_sdl_opengl3
```

To add the library to your project simply add to your CMakeLists.txt: 

```add_subdirectory("path/to/imgui-cmake")```

then link against the provided target: 

```target_link_libraries(YourTarget IMGUI)```

# Raspberry support

For more accurate information refer to [this](https://github.com/ocornut/imgui/pull/2837) thread.

Some important things to note if you are building for raspberry:
- On raspberry to get decent performance we have to use the GPU with OpenGLES. On Pi3 or older this means running the program at fullscreen. I don't know why but i cant get it working windowed (i haven't done extensive testing though). On Pi4 it should work in both modes.
- On Raspberry there's a bug with the latest versions of the gcc compiler. Basically the `<limits.h>` file has been modified, and the `PATH_MAX` macro is no longer there. 
This causes an error when compiling `SDL/src/haptic/linux/SDL_syshaptic.c`. To fix this make sure you are using gcc-4.9 or an **earlier** version. When running cmake specify: `-D CMAKE_C_COMPILER=gcc-4.9 -D CMAKE_CXX_COMPILER=g++-4.9`. An alternative would be to add `#define PATH_MAX 4096` at the beginning of the SDL file, although this is not recommended since it modifies the library.

On a Raspberry Pi 3 the performance is around 100-130 fps at 1080p for the imgui demo. It can drop down to as low as 30 fps or even less if certain demanding features are being displayed. One example of such features are the colored buttons present in the demo under `layout & scrolling -> scrolling`.

When using the broadcom drivers you may have to ```export SDL_VIDEODRIVER=rpi``` before running the program.

# About the example

- `example_sdl_opengl3`: exactly the same as the original imgui example. 
