cmake_minimum_required(VERSION 3.1)
project(game.libretro.nestopia)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

find_package(Kodi REQUIRED)
find_package(libretro-nestopia REQUIRED)

set(DEPLIBS nestopia)
set(NESTOPIA_CUSTOM_BINARY ${NESTOPIA_LIB}
                           ${PROJECT_NAME}${CMAKE_SHARED_LIBRARY_SUFFIX})

build_addon(${PROJECT_NAME} NESTOPIA DEPLIBS)
