cmake_minimum_required(VERSION 2.80)

project(emhash_bench)

if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

set(CMAKE_CXX_STANDARD 14)

message("------------ Options -------------")
message("  CMAKE_BUILD_TYPE: ${CMAKE_BUILD_TYPE}")
message("  CMAKE_SYSTEM_NAME: ${CMAKE_SYSTEM_NAME}")
message("  CMAKE_SYSTEM_PROCESSOR: ${CMAKE_SYSTEM_PROCESSOR} ")
message("  CMAKE_CXX_COMPILER_ID: ${CMAKE_CXX_COMPILER_ID} ${CMAKE_CXX_COMPILER_VERSION}")

include_directories(${PROJECT_SOURCE_DIR})
#include_directories(${PROJECT_SOURCE_DIR}/emhash)
include_directories(${PROJECT_SOURCE_DIR}/bench)

add_definitions(-DET=1 -DTKey=1 -DTVal=1)

if(WIN32)
    set(CMAKE_CXX_FLAGS "/WX- /MP")
    set(CMAKE_CXX_FLAGS_DEBUG "/W3 /Zi /Od /WX- ${CMAKE_CXX_FLAGS}")
    set(CMAKE_CXX_FLAGS_RELEASE "/Ob1 /Ot /Oi /Oy /GL /arch:AVX ${CMAKE_CXX_FLAGS}")
    add_compile_options(/Ob2 /DNDEBUG /O2 /Ot /Oi /Oy /GL /arch:AVX)
elseif()
    set(CMAKE_CXX_FLAGS_DEBUG "-g -fno-strict-aliasing ${CMAKE_CXX_FLAGS}")
    set(CMAKE_CXX_FLAGS_RELEASE "-DNDEBUG -O3 -march=native -fno-strict-aliasing ${CMAKE_CXX_FLAGS}")
endif()

add_executable(ebench ${PROJECT_SOURCE_DIR}/bench/ebench.cpp)
add_executable(mbench ${PROJECT_SOURCE_DIR}/bench/martin_bench.cpp)
add_executable(hbench ${PROJECT_SOURCE_DIR}/bench/hbench.cpp)

