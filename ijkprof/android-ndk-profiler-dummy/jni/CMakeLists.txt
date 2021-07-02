cmake_minimum_required(VERSION 3.6)

set(CMAKE_POSITION_INDEPENDENT_CODE ON)

add_library(android-ndk-profiler STATIC prof.c)
