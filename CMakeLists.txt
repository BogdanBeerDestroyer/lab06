cmake_minimum_required(VERSION 3.4)
project(formatter)
set(SOLV_VERSION_MAJOR 0)
set(SOLV_VERSION_MINOR 1)
set(SOLV_VERSION_PATCH 0)
set(SOLV_VERSION_TWEAK 0)
set(SOLV_VERSION
  ${SOLV_VERSION_MAJOR}.${SOLV_VERSION_MINOR}.${SOLV_VERSION_PATCH}.${SOLV_VERSION_TWEAK})
set(SOLV_VERSION_STRING "v${SOLV_VERSION}")

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter STATIC formatter_lib/formatter.cpp)
include_directories(formatter_lib)

add_library(formatter_ex STATIC formatter_ex_lib/formatter_ex.cpp)
target_link_libraries(formatter_ex formatter)
include_directories(formatter_ex_lib)

add_executable(hello_world hello_world_application/hello_world.cpp)
target_link_libraries(hello_world formatter formatter_ex)

add_library(solver_lib STATIC solver_lib/solver.cpp)
include_directories(solver_lib)

add_executable(solver solver_application/equation.cpp)
target_link_libraries(solver formatter formatter_ex solver_lib)

include(CPackConfig.cmake)
