# Copyright (c) 2023-2024 Christian Hinkle, Brian Hinkle.

cmake_minimum_required(VERSION 3.24)

project(MeddySDKSuperbuildStandalone
  LANGUAGES CXX
  )

include(ExternalProject)

# Build dependencies first.

ExternalProject_Add(Boost
  SOURCE_DIR "${MeddySDKSuperbuildStandalone_SOURCE_DIR}/boost"
  CMAKE_ARGS -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE} -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX} -DCMAKE_PREFIX_PATH=${CMAKE_PREFIX_PATH}
  )

# Now, after all the depenencies are finished building, start the superbuild.

#
# Run the superbuild subproject.
#
# TODO: It would make more sense to add MeddySDKSuperbuild as a subproject with FetchContent instead of ExternalProject. But
# I don't know how to have a FetchContent subproject wait for an ExternalProject to finish.
#
ExternalProject_Add(MeddySDKSuperbuild
  DEPENDS Boost
  SOURCE_DIR "${MeddySDKSuperbuildStandalone_SOURCE_DIR}/MeddySDKSuperbuild"
  CMAKE_ARGS -DCMAKE_BUILD_TYPE=${CMAKE_BUILD_TYPE} -DCMAKE_INSTALL_PREFIX=${CMAKE_INSTALL_PREFIX} -DCMAKE_PREFIX_PATH=${CMAKE_PREFIX_PATH}
  )
