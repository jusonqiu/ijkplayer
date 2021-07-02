cmake_minimum_required(VERSION 3.6)
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99")

set(IJK_SDL_SOURCES
        ijksdl_aout.c
        ijksdl_audio.c
        ijksdl_egl.c
        ijksdl_error.c
        ijksdl_mutex.c
        ijksdl_stdinc.c
        ijksdl_thread.c
        ijksdl_timer.c
        ijksdl_vout.c
        ijksdl_extra_log.c
        gles2/color.c
        gles2/common.c
        gles2/renderer.c
        gles2/renderer_rgb.c
        gles2/renderer_yuv420p.c
        gles2/renderer_yuv444p10le.c
        gles2/shader.c
        gles2/fsh/rgb.fsh.c
        gles2/fsh/yuv420p.fsh.c
        gles2/fsh/yuv444p10le.fsh.c
        gles2/vsh/mvp.vsh.c

        dummy/ijksdl_vout_dummy.c
        ffmpeg/ijksdl_vout_overlay_ffmpeg.c
        ffmpeg/abi_all/image_convert.c
        android/android_audiotrack.c
        android/android_nativewindow.c
        android/ijksdl_android_jni.c
        android/ijksdl_aout_android_audiotrack.c
        android/ijksdl_aout_android_opensles.c
        android/ijksdl_codec_android_mediacodec_dummy.c
        android/ijksdl_codec_android_mediacodec_internal.c
        android/ijksdl_codec_android_mediacodec_java.c
        android/ijksdl_codec_android_mediacodec.c
        android/ijksdl_codec_android_mediadef.c
        android/ijksdl_codec_android_mediaformat_java.c
        android/ijksdl_codec_android_mediaformat.c
        android/ijksdl_vout_android_nativewindow.c
        android/ijksdl_vout_android_surface.c
        android/ijksdl_vout_overlay_android_mediacodec.c)


set(IJKSDL_LIB_NAME ijksdl)
add_library(${IJKSDL_LIB_NAME} SHARED ${IJK_SDL_SOURCES})
target_include_directories(${IJKSDL_LIB_NAME} PRIVATE ${CMAKE_CURRENT_LIST_DIR})
target_include_directories(${IJKSDL_LIB_NAME} PRIVATE ${CMAKE_CURRENT_LIST_DIR}/..)

#include ffmpeg header files
target_include_directories(${IJKSDL_LIB_NAME} PRIVATE ${IJK_FFMPAG_BUILD_DIR}/include)

#target_compile_options(${IJKSDL_LIB_NAME} PUBLIC "-Werror")
target_compile_options(${IJKSDL_LIB_NAME} PRIVATE "-Wno-implicit-function-declaration")
target_compile_options(${IJKSDL_LIB_NAME} PRIVATE "-Wno-deprecated-declarations")

if (ANDROID)
    # build cpufeatures as a static lib
    # add_library(cpufeatures STATIC
    #        ${ANDROID_NDK}/sources/android/cpufeatures/cpu-features.c)

    # include(AndroidNdkModules)
    # android_ndk_import_module_cpufeatures()
    target_include_directories(${IJKSDL_LIB_NAME} PRIVATE ${CMAKE_CURRENT_LIST_DIR}/../ijkyuv/include)
    target_include_directories(${IJKSDL_LIB_NAME} PRIVATE ${CMAKE_CURRENT_LIST_DIR}/../ijkj4a)
    target_link_libraries(${IJKSDL_LIB_NAME} ijkffmpeg)
    target_link_libraries(${IJKSDL_LIB_NAME} ijkj4a)
#     target_link_libraries(${IJKSDL_LIB_NAME} cpufeatures) #cpu 优化
    target_link_libraries(${IJKSDL_LIB_NAME} yuv_static)
    target_link_libraries(${IJKSDL_LIB_NAME} log android OpenSLES EGL GLESv2)
endif ()

