cmake_minimum_required(VERSION 3.16)

project(RDMAtest)
set(CMAKE_CXX_COMPILER "/usr/bin/g++")
add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/src/network)

add_executable (rdmaTest ${CMAKE_CURRENT_SOURCE_DIR}/main.cpp)

target_link_libraries(rdmaTest PRIVATE network)
target_compile_features(rdmaTest PRIVATE cxx_std_17)