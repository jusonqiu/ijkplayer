cmake_minimum_required(VERSION 3.6)

#configure_file(ijkplayer/ijkversion.h.in ${CMAKE_CURRENT_LIST_DIR}/ijkplayer/ijkversion.h @ONLY)

if (ANDROID)
    add_subdirectory(ijksoundtouch)
    add_subdirectory(ijkyuv)
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