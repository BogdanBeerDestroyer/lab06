cmake_minimum_required(VERSION 3.4)
project(formatter_ex)
include_directories(formatter_lib) 
add_subdirectory(formatter_lib) #она соберется сама т.к в ней есть собственный CMakeList
add_library(formatter_ex STATIC formatter_ex.h formatter_ex.cpp)
target_link_libraries(formatter_ex formatter) 
