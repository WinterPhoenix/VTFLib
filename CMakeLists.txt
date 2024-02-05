cmake_minimum_required(VERSION 3.16)
cmake_policy(SET CMP0091 NEW)

project(vtflib C CXX)

option(VTFLIB_STATIC "If enabled, builds VTFLIB as a static library" OFF)
option(VTFLIB_SHARED "If enabled, builds VTFLIB as a shared library" ON)

set(CMAKE_CXX_STANDARD 17)

if (UNIX)
	set(CMAKE_SHARED_LINKER_FLAGS "${CMAKE_SHARED_LINKER_FLAGS} -Wl,--no-undefined")
endif()

# MT/MTd specification for Windows
set(CMAKE_MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>")
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

add_subdirectory(VTFLib)
add_subdirectory(VTFCmd)
