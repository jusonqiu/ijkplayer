cmake_minimum_required(VERSION 3.6)

if (ANDROID)
    if (${ANDROID_ABI} STREQUAL "armeabi-v7a")
        set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -mfloat-abi=soft")
    endif ()
endif ()

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99")

set(BUILD_SOURCES
    ff_cmdutils.c
    ff_ffplay.c
    ff_ffpipeline.c
    ff_ffpipenode.c

    ijkmeta.c
    ijkplayer.c

    pipeline/ffpipeline_ffplay.c
    pipeline/ffpipenode_ffplay_vdec.c
    android/ffmpeg_api_jni.c
    android/ijkplayer_android.c
    android/ijkplayer_jni.c
    android/pipeline/ffpipeline_android.c
    android/pipeline/ffpipenode_android_mediacodec_vdec.c
    ijkavformat/allformats.c
    ijkavformat/ijklivehook.c
    ijkavformat/ijkmediadatasource.c
    ijkavformat/ijkio.c
    ijkavformat/ijkiomanager.c
    ijkavformat/ijkiocache.c
    ijkavformat/ijkioffio.c
    ijkavformat/ijkioandroidio.c
    ijkavformat/ijkioprotocol.c
    ijkavformat/ijkioapplication.c
    ijkavformat/ijkiourlhook.c

    ijkavformat/ijkasync.c
    ijkavformat/ijkurlhook.c
    ijkavformat/ijklongurl.c
    ijkavformat/ijksegment.c
    ijkavutil/ijkdict.c
    ijkavutil/ijkutils.c
    ijkavutil/ijkthreadpool.c
    ijkavutil/ijktree.c
    ijkavutil/ijkfifo.c
    ijkavutil/ijkstl.cpp
)

set(IJKPLAYER_LIB_NAME ijkplayer)

add_library(${IJKPLAYER_LIB_NAME} SHARED ${BUILD_SOURCES})

target_include_directories(${IJKPLAYER_LIB_NAME} PRIVATE ${CMAKE_CURRENT_LIST_DIR})
target_include_directories(${IJKPLAYER_LIB_NAME} PRIVATE ${CMAKE_CURRENT_LIST_DIR}/..)
target_include_directories(${IJKPLAYER_LIB_NAME} PRIVATE ${IJK_FFMPAG_BUILD_DIR}/include)
target_include_directories(${IJKPLAYER_LIB_NAME} PRIVATE ijkavformat)
target_include_directories(${IJKPLAYER_LIB_NAME} PRIVATE ijkavutil)
target_include_directories(${IJKPLAYER_LIB_NAME} PRIVATE pipeline)
target_include_directories(${IJKPLAYER_LIB_NAME} PRIVATE ${CMAKE_CURRENT_LIST_DIR}/../ijkj4a)

if (ANDROID)
    target_link_libraries(${IJKPLAYER_LIB_NAME} ijksoundtouch)
    target_link_libraries(${IJKPLAYER_LIB_NAME} ijksdl)
    target_link_libraries(${IJKPLAYER_LIB_NAME} ijkffmpeg)
    target_link_libraries(${IJKPLAYER_LIB_NAME} android-ndk-profiler)
    target_link_libraries(${IJKPLAYER_LIB_NAME} log)
    target_link_libraries(${IJKPLAYER_LIB_NAME} android)
endif ()

