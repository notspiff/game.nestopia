project(game.libretro.nestopia)

cmake_minimum_required(VERSION 2.6)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

enable_language(CXX)

find_package(kodi REQUIRED)
find_package(libretro-nestopia REQUIRED)

set(DEPLIBS nestopia)
set(NESTOPIA_CUSTOM_BINARY ${NESTOPIA_LIB}
                           game.libretro.nestopia${CMAKE_SHARED_LIBRARY_SUFFIX} nestopia)

build_addon(game.libretro.nestopia NESTOPIA DEPLIBS)
