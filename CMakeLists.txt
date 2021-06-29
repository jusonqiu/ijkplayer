cmake_minimum_required(VERSION 3.6)

project("vcore")

if (${CMAKE_BUILD_TYPE} STREQUAL "Release")
    add_definitions(-DNDEBUG)
elseif (${CMAKE_BUILD_TYPE} STREQUAL "Debug")
    add_definitions(-DDEBUG)
endif ()


# add_compile_options("-Wno-deprecated-declarations")
# set(CMAKE_POSITION_INDEPENDENT_CODE ON)

set(ANDROID_CONTRIB_PATH ${FIJKROOT_PATH}/android/contrib/build)
message(STATUS "ANDROID_CONTRIB_PATH:${ANDROID_CONTRIB_PATH}")

add_subdirectory(ijkmedia)