project(OpenTLD)
cmake_minimum_required(VERSION 2.6)

#-------------------------------------------------------------------------------
#version
set(TLD_VERSION_MAJOR 1)
set(TLD_VERSION_MINOR 4)
set(TLD_VERSION_PATCH 0)
set(TLD_VERSION ${TLD_VERSION_MAJOR}.${TLD_VERSION_MINOR}.${TLD_VERSION_PATCH})

add_subdirectory(lib)
add_subdirectory(examples)
