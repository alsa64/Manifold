cmake_minimum_required(VERSION 3.15)
project(Manifold)

list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_LIST_DIR}/cmake")

# C++ standard and compile flags now set per-target below

# GNU-specific flags handled per-target below

option(BUILD_PERFORMANCE_TEST "More subdivisition for performance test" OFF)
option(BUILD_OPENMP "Enable support for OpenMP" OFF)
option(BUILD_LOG "Enable verbose log" OFF)

include_directories(src)
include_directories(3rd)

# Compile definitions now set per-target below

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

# GNU-specific compile and link options
if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
  target_compile_options(manifold PRIVATE
    -Wno-int-in-bool-context
    -Wsign-compare
    $<$<CONFIG:Debug>:-fsanitize=address>
  )
  target_compile_options(simplify PRIVATE
    -Wno-int-in-bool-context
    -Wsign-compare
    $<$<CONFIG:Debug>:-fsanitize=address>
  )

  target_link_options(manifold PRIVATE
    $<$<CONFIG:Debug>:-fsanitize=address>
  )
  target_link_options(simplify PRIVATE
    $<$<CONFIG:Debug>:-fsanitize=address>
  )
endif()

# Target-specific compile definitions
target_compile_definitions(manifold PRIVATE
  $<$<BOOL:${BUILD_PERFORMANCE_TEST}>:PERFORMANCE_TEST>
  $<$<BOOL:${BUILD_OPENMP}>:WITH_OMP>
  $<$<BOOL:${BUILD_LOG}>:LOG_OUTPUT>
)

target_compile_definitions(simplify PRIVATE
  $<$<BOOL:${BUILD_LOG}>:LOG_OUTPUT>
)

