project(game.nestopia)

cmake_minimum_required(VERSION 2.6)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

enable_language(CXX)

find_package(kodi REQUIRED)

include(ExternalProject)

set(DEBUG_FLAG DEBUG=1)

if("${CORE_SYSTEM_NAME}" STREQUAL "windows")
  set(BUILD_COMMAND mingw32-make -C libretro platform=win ${DEBUG_FLAG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "linux")
  set(BUILD_COMMAND make -C libretro ${DEBUG_FLAG})
elseif("${CORE_SYSTEM_NAME}" STREQUAL "darwin")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "ios")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "android")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "rbpi")
  # TODO
elseif("${CORE_SYSTEM_NAME}" STREQUAL "freebsd")
# TODO
endif()

externalproject_add(nestopia
                    GIT_REPOSITORY https://github.com/libretro/nestopia
                    GIT_TAG c956d46
                    PREFIX nestopia
                    CONFIGURE_COMMAND ""
                    INSTALL_COMMAND ""
                    BUILD_COMMAND ${BUILD_COMMAND}
                    BUILD_IN_SOURCE 1)

set(NESTOPIA_CUSTOM_BINARY ${PROJECT_BINARY_DIR}/nestopia/src/nestopia/libretro/nestopia_libretro${CMAKE_SHARED_LIBRARY_SUFFIX}
                           nestopia_libretro${CMAKE_SHARED_LIBRARY_SUFFIX} nestopia)

build_addon(game.nestopia NESTOPIA DEPLIBS)
add_dependencies(game.nestopia nestopia)
SET_TARGET_PROPERTIES(game.nestopia PROPERTIES LINKER_LANGUAGE CXX)
