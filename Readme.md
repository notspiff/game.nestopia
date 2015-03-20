# game.nestopia

Nestopia is a portable open source NES/Famicom emulator. It is designed to be as accurate as possible and supports a large number of peripherals. The hardware is emulated at cycle-by-cycle granularity, ensuring full support for software that does mid-scanline and other timing trickery.

# Building out-of-tree (recommended)

## Linux

Clone the repo and create a build directory

```shell
git clone https://github.com/kodi-game/game.nestopia.git
cd game.nestopia
mkdir build
cd build
```

Generate a build environment with config for debugging

```shell
cmake -DADDONS_TO_BUILD=game.nestopia \
      -DADDON_SRC_PREFIX=$HOME/workspace \
      -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_INSTALL_PREFIX=$HOME/workspace/xbmc/addons \
      -DPACKAGE_ZIP=1 \
      $HOME/workspace/xbmc/project/cmake/addons
```

If you are developing in Eclipse, you can create a "makefile project with existing code" using `game.nestopia/` as the existing code location. To build, enter Properties -> "C/C++ Build" and change the build command to `make -C build`.

It is also possible to generate Eclipse project files with cmake

```shell
cmake -G"Eclipse CDT4 - Unix Makefiles" \
      -D_ECLIPSE_VERSION=4.4 \
      -DADDONS_TO_BUILD=game.nestopia \
      -DADDON_SRC_PREFIX=$HOME/workspace \
      -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_INSTALL_PREFIX=$HOME/workspace/xbmc/addons \
      -DPACKAGE_ZIP=1 \
      $HOME/workspace/xbmc/project/cmake/addons
```

## Windows

First, download and install [CMake](http://www.cmake.org/download/) and [MinGW](http://www.mingw.org/). Add the MinGW `bin` folder to your path (e.g. `C:\MinGW\bin`).

Run the script from [PR 6658](https://github.com/xbmc/xbmc/pull/6658) to create Visual Studio project files

```
tools\windows\prepare-binary-addons-dev.bat
```

The generated solution can be found at

```
project\cmake\addons\build\kodi-addons.sln
```

Currently, the build system corrupts `addons/game.nestopia/game.nestopia.dll`. You can find the correct .dll here

```
project\cmake\addons\build\game.nestopia-prefix\src\game.nestopia-build\nestopia\src\nestopia\libretro\nestopia_libretro.dll
```

Copy this to `addons/game.nestopia/` and rename to `game.libretro.dll` to match the DLL name in [addon.xml](https://github.com/kodi-game/game.nestopia/blob/master/game.nestopia/addon.xml).

# Building in-tree (cross-compiling)

Kodi's build system will fetch the add-on from the GitHub URL and git hash specified in [game.nestopia.txt](https://github.com/garbear/xbmc/blob/retroplayer-15alpha2/project/cmake/addons/addons/game.nestopia/game.nestopia.txt).

## Windows

Remember, CMake and an external MinGW are required.

```shell
cd tools\buildsteps\win32
make-addons.bat game.nestopia
```

See above for the location of the correct .dll.

## OSX

Per [README.osx](https://github.com/garbear/xbmc/blob/retroplayer-15alpha2/docs/README.osx), enter the `tools/depends` directory and make the add-on:

```shell
cd tools/depends
make -C target/binary-addons ADDONS="game.nestopia"
```
Currently, the build system corrupts `addons/game.nestopia/game.nestopia.dylib`. You can find the correct .dylib here

```
tools/depends/target/binary-addons/macosx10.10_x86_64-target/game.nestopia-prefix/src/game.nestopia-build/nestopia/src/nestopia/libretro/nestopia_libretro.dylib
```

The solution I've found is to symlink to the correct .dylib

```shell
cd addons/game.nestopia/
rm game.nestopia.*
ln -s ../../tools/depends/target/binary-addons/macosx10.10_x86_64-target/game.nestopia-prefix/src/game.nestopia-build/nestopia/src/nestopia/libretro/nestopia_libretro.dylib   game.nestopia.dylib
```

Unfortunately the symlink gets overwritten when the add-on is rebuilt.

## Cleaning build directory

Run the following to clean the build directory. Note, this will clean all add-ons, not just game.nestopia.

```shell
make -C target/binary-addons clean
```
