cmake_minimum_required(VERSION 3.6)

#configure_file(ijkplayer/ijkversion.h.in ${CMAKE_CURRENT_LIST_DIR}/ijkplayer/ijkversion.h @ONLY)

### ijkyuv start ###
set(YUV_LIB "ijkyuv")
set(YUV_SRC_DIR ${CMAKE_CURRENT_LIST_DIR}/ijkyuv)
set(YUV_SRCS
        ${YUV_SRC_DIR}/source/compare.cc
        ${YUV_SRC_DIR}/source/compare_common.cc
        ${YUV_SRC_DIR}/source/compare_posix.cc
        ${YUV_SRC_DIR}/source/convert.cc
        ${YUV_SRC_DIR}/source/convert_argb.cc
        ${YUV_SRC_DIR}/source/convert_from.cc
        ${YUV_SRC_DIR}/source/convert_from_argb.cc
        ${YUV_SRC_DIR}/source/convert_to_argb.cc
        ${YUV_SRC_DIR}/source/convert_to_i420.cc
        ${YUV_SRC_DIR}/source/cpu_id.cc
        ${YUV_SRC_DIR}/source/format_conversion.cc
        ${YUV_SRC_DIR}/source/planar_functions.cc
        ${YUV_SRC_DIR}/source/rotate.cc
        ${YUV_SRC_DIR}/source/rotate_argb.cc
        ${YUV_SRC_DIR}/source/rotate_mips.cc
        ${YUV_SRC_DIR}/source/row_any.cc
        ${YUV_SRC_DIR}/source/row_common.cc
        ${YUV_SRC_DIR}/source/row_mips.cc
        ${YUV_SRC_DIR}/source/row_posix.cc
        ${YUV_SRC_DIR}/source/scale.cc
        ${YUV_SRC_DIR}/source/scale_argb.cc
        ${YUV_SRC_DIR}/source/scale_common.cc
        ${YUV_SRC_DIR}/source/scale_mips.cc
        ${YUV_SRC_DIR}/source/scale_posix.cc
        ${YUV_SRC_DIR}/source/video_common.cc
)

if (${CMAKE_ANDROID_ARCH_ABI} STREQUAL "armeabi-v7a")
list(APPEND YUV_SRCS
        ${YUV_SRC_DIR}/source/compare_neon.cc.neon
        ${YUV_SRC_DIR}/source/rotate_neon.cc.neon
        ${YUV_SRC_DIR}/source/row_neon.cc.neon
        ${YUV_SRC_DIR}/source/scale_neon.cc.neon)
elseif (${CMAKE_ANDROID_ARCH_ABI} STREQUAL "arm64-v8a")
list(APPEND YUV_SRCS
        ${YUV_SRC_DIR}/source/compare_neon64.cc
        ${YUV_SRC_DIR}/source/rotate_neon64.cc
        ${YUV_SRC_DIR}/source/row_neon64.cc
        ${YUV_SRC_DIR}/source/scale_neon64.cc)
endif ()

add_library(${YUV_LIB} STATIC ${YUV_SRCS})
target_include_directories(${YUV_LIB} PRIVATE ${YUV_SRC_DIR}/include)
### ijkyun end ###


if (ANDROID)
    add_subdirectory(ijksoundtouch)
#    add_subdirectory(ijkyuv)
    add_subdirectory(ijkj4a)
    # just a dummy android ndk prifiler
    option(ANDROID_NDK_PROFILER OFF)
    if (NDK_PROFILER)
        # not implemented yet
        message(FATAL_ERROR "not implemented yet, android-ndk-profiler")
        add_subdirectory(${IJK_ROOT}../ijkprof/android-ndk-profiler-dummy/jni/ ijkprof)
    else ()
        add_subdirectory(${IJK_ROOT}/ijkprof/android-ndk-profiler-dummy/jni/ ijkprof)
    endif (NDK_PROFILER)

    # add_dependencies(ijkplayer android-ndk-profiler)
endif ()

add_subdirectory(ijksdl)
add_subdirectory(ijkplayer)