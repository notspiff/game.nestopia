project(game.nestopia)

cmake_minimum_required(VERSION 2.6)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

enable_language(CXX)

find_package(kodi REQUIRED)

include(ExternalProject)

externalproject_add(nestopia
                    GIT_REPOSITORY https://github.com/libretro/nestopia
                    GIT_TAG 78b403c
                    PREFIX nestopia
                    CONFIGURE_COMMAND ""
                    INSTALL_COMMAND ""
                    BUILD_COMMAND make -C libretro
                    BUILD_IN_SOURCE 1)

set(NESTOPIA_CUSTOM_BINARY ${PROJECT_BINARY_DIR}/nestopia/src/nestopia/libretro/nestopia_libretro${CMAKE_SHARED_LIBRARY_SUFFIX}
                           nestopia_libretro${CMAKE_SHARED_LIBRARY_SUFFIX} nestopia)

build_addon(game.nestopia NESTOPIA DEPLIBS)
add_dependencies(game.nestopia nestopia)
SET_TARGET_PROPERTIES(game.nestopia PROPERTIES LINKER_LANGUAGE CXX)
