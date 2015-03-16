# Building out-of-tree (recommended)

## Linux

Clone and enter the repo and create a build folder

```shell
git clone https://github.com/kodi-game/game.nestopia.git
cd game.nestopia
mkdir build
cd build
```

Generate a build environment with config for debugging

```shell

cmake -DADDONS_TO_BUILD=game.nestopia \
      -DADDON_SRC_PREFIX=/home/xbmc/progs/src \
      -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_INSTALL_PREFIX=/home/xbmc/progs/src/xbmc/addons \
      -DPACKAGE_ZIP=1 \
      /home/xbmc/progs/src/xbmc/project/cmake/addons
```

If you are developing in Eclipse, you can create a "makefile project with existing code" using `game.nestopia/` as the existing code location. To build, enter Properties -> "C/C++ Build" and change the build command to `make -C build`.

It is also possible to generate Eclipse project files with cmake

```shell
cmake -G"Eclipse CDT4 - Unix Makefiles" \
      -D_ECLIPSE_VERSION=4.4 \
      -DADDONS_TO_BUILD=game.nestopia \
      -DADDON_SRC_PREFIX=/home/xbmc/progs/src \
      -DCMAKE_BUILD_TYPE=Debug \
      -DCMAKE_INSTALL_PREFIX=/home/xbmc/progs/src/xbmc/addons \
      -DPACKAGE_ZIP=1 \
      /home/xbmc/progs/src/xbmc/project/cmake/addons
```

## Windows

First, download and install [CMake](http://www.cmake.org/download/) and [MinGW](http://www.mingw.org/). Add the MinGW `bin` folder to your path (e.g. `C:\MinGW\bin`).

To build on windows, change to the addons folder:

```batch
cd D:\Projects\xbmx\xbmc\project\cmake\addons
```

Generate Visual Studio 2013 solution

```batch
cmake -DADDONS_TO_BUILD=game.nestopia -DADDON_SRC_PREFIX="D:\Projects\demo" -DCMAKE_BUILD_TYPE=Debug -G "Visual Studio 12"  -DCMAKE_USER_MAKE_RULES_OVERRIDE="D:\Projects\xbmx\xbmc\project\cmake\scripts\windows\c-flag-overrides.cmake" -DCMAKE_USER_MAKE_RULES_OVERRIDE_CXX="D:\Projects\xbmx\xbmc\project\cmake\scripts\windows\cxx-flag-overrides.cmake" -DCMAKE_INSTALL_PREFIX="D:\Projects\xbmx\xbmc\adons" -DPACKAGE_ZIP=1
```

Open Visual Studio, load and build this solution:

```
D:\Projects\xbmx\xbmc\project\cmake\addons\kodi-addons.sln
```

Alternatively, wait for the `prepare-addons-dev.bat` build script in [PR #6658](https://github.com/xbmc/xbmc/pull/6658) to be merged. Enter tools/buildsteps/win32 and execute it from there. If you want to execute it from somewhere else you need to adjust the default value of WORKDIR in the batch file.

Available options are:
* **clean** to simply clean the whole generated buildsystem
* **&lt;addon-id>** to only generate the buildsystem for that addon

# Building in-tree

Kodi's build system will fetch the add-on from the GitHub URL and git hash specified in [game.nestopia.txt](https://github.com/garbear/xbmc/blob/retroplayer-15alpha2/project/cmake/addons/addons/game.nestopia/game.nestopia.txt).

## Windows

Remember, CMake and an external MinGW are required.

```shell
cd tools\buildsteps\win32
make-addons.bat game.nestopia
```

The compiled .dll will be placed in `project\cmake\addons\build\game.nestopia-prefix\src\game.nestopia-build\nestopia\src\nestopia\libretro`. You will need to rename this DLL to `game.nestopia.dll` to match the DLL name in [addon.xml](https://github.com/kodi-game/game.nestopia/blob/master/game.nestopia/addon.xml).

## OSX

Per [README.osx](https://github.com/garbear/xbmc/blob/retroplayer-15alpha2/docs/README.osx), enter the `tools/depends` directory and make the add-on:

```shell
cd tools/depends
make -C target/binary-addons ADDONS="game.nestopia"
```

The .dylib installed to `addons/` is corrupted. You will need to manually copy the .dylib from `tools/depends/target/binary-addons/macosx10.10_x86_64-target/game.nestopia-prefix/src/game.nestopia-build/nestopia/src/nestopia/libretro/nestopia_libretro.dylib`.

## Cleaning build directory

Run the following to clean the build directory. Note, this will clean all add-ons, not just game.nestopia.

```shell
make -C target/binary-addons clean
```
