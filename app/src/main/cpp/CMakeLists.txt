cmake_minimum_required(VERSION 3.10)
project(edge_detect_native)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# path to your cpp sources (adjust if your cpp file path differs)
add_library(native-lib SHARED native-lib.cpp)

# include OpenCV headers (if you have OpenCV headers under app/src/main/cpp/include)
include_directories(${CMAKE_SOURCE_DIR}/include)

# Import the combined OpenCV Android JNI library that you already have in jniLibs
# Note: CMAKE_SOURCE_DIR here is typically the cpp directory (e.g. app/src/main/cpp).
# The ../jniLibs path mirrors your previous layout; using ${ANDROID_ABI} makes it ABI-agnostic.
add_library(opencv_java4 SHARED IMPORTED)
set_target_properties(opencv_java4 PROPERTIES
        IMPORTED_LOCATION "${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI}/libopencv_java4.so"
)

# Find Android log library
find_library(log-lib log)

# Link libraries to your native target
target_link_libraries(
        native-lib
        PRIVATE
        opencv_java4
        ${log-lib}
)
