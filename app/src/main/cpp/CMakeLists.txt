cmake_minimum_required(VERSION 3.4.1)

# configure import libs
set(distribution_DIR ${CMAKE_SOURCE_DIR}/../../../../ffmpeg-android)

add_library(lib_avformat STATIC IMPORTED)
set_target_properties(lib_avformat PROPERTIES IMPORTED_LOCATION
    ${distribution_DIR}/${ANDROID_ABI}/lib/libavformat.a)

add_library(lib_avcodec STATIC IMPORTED)
set_target_properties(lib_avcodec PROPERTIES IMPORTED_LOCATION
    ${distribution_DIR}/${ANDROID_ABI}/lib/libavcodec.a)

add_library(lib_swscale STATIC IMPORTED)
set_target_properties(lib_swscale PROPERTIES IMPORTED_LOCATION
    ${distribution_DIR}/${ANDROID_ABI}/lib/libswscale.a)
add_library(lib_avutil STATIC IMPORTED)
set_target_properties(lib_avutil PROPERTIES IMPORTED_LOCATION
    ${distribution_DIR}/${ANDROID_ABI}/lib/libavutil.a)	
# now build app's shared lib
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -Wall")

add_library(gl2jni SHARED
            gl_code.cpp vdecoder.cpp RingBuffer.cpp)
target_include_directories(gl2jni PRIVATE
	${distribution_DIR}/include
	)

# add lib dependencies
target_link_libraries(gl2jni
                      android
                      log 
                      EGL
					  lib_avformat
					  lib_avcodec
					  lib_swscale
					  lib_avutil
                      GLESv2)
