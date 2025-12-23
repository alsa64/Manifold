cmake_minimum_required(VERSION 3.15)
project(Manifold)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")

# C++ standard and compile flags now set per-target below

if ("${CMAKE_CXX_COMPILER_ID}" STREQUAL "GNU")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wno-int-in-bool-context -Wsign-compare")
    set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_CXX_FLAGS_DEBUG} -fsanitize=address")
    set(CMAKE_LINKER_FLAGS "${CMAKE_LINKER_FLAGS}")
    set(CMAKE_LINKER_FLAGS_DEBUG "${CMAKE_LINKER_FLAGS_DEBUG} -fsanitize=address")
endif()

option(BUILD_PERFORMANCE_TEST "More subdivisition for performance test" OFF)
option(BUILD_OPENMP "Enable support for OpenMP" OFF)
option(BUILD_LOG "Enable verbose log" OFF)

include_directories(src)
include_directories(3rd)

if(BUILD_PERFORMANCE_TEST)
    add_definitions(-DPERFORMANCE_TEST)
endif(BUILD_PERFORMANCE_TEST)

if(BUILD_OPENMP)
    add_definitions(-DWITH_OMP)
endif(BUILD_OPENMP)

if(BUILD_LOG)
    add_definitions(-DLOG_OUTPUT)
endif(BUILD_LOG)

set(
    manifold_SRC
    src/BVH.cpp
    src/BVH.h
    src/Intersection.cpp
    src/Intersection.h
    src/main.cpp
    src/Model_OBJ.cpp
    src/Model_OBJ.h
    src/Octree.h    
)

set(
    simplify_SRC
    src/simplify.cpp
)

add_executable(
    manifold
    ${manifold_SRC}
)

add_executable(
    simplify
    ${simplify_SRC}
)

# Target-specific C++ standard
target_compile_features(manifold PRIVATE cxx_std_11)
target_compile_features(simplify PRIVATE cxx_std_11)

# Target-specific compile options
target_compile_options(manifold PRIVATE
  -Wall
  -pthread
  $<$<CONFIG:Release>:-O3>
)

target_compile_options(simplify PRIVATE
  -Wall
  -pthread
  $<$<CONFIG:Release>:-O3>
)

