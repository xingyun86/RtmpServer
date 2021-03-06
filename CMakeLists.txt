cmake_minimum_required(VERSION 2.6)

project(RtmpeServer)

add_subdirectory(${PROJECT_SOURCE_DIR}/../CoSocket/CoSocket CoSocket)

set(CMAKE_C_FLAGS
    "-Wall -Wextra -Wno-unused-parameter")

set(CMAKE_C_FLAGS_DEBUG
    "-g")

set(CMAKE_C_FLAGS_RELEASE
    "-O3 -g -DRELEASE -DNDEBUG")

set(CMAKE_CXX_FLAGS
    "-Wall -Wextra -Wno-unused-parameter -std=c++11")

set(CMAKE_CXX_FLAGS_DEBUG
    "-g")

set(CMAKE_CXX_FLAGS_RELEASE
    "-O3 -g -DRELEASE -DNDEBUG")

set(CMAKE_EXE_LINKER_FLAGS
    "-Wl,-rpath=.:..:lib")

set(CMAKE_USE_RELATIVE_PATHS TRUE)

set(EXECUTABLE_OUTPUT_PATH "${PROJECT_BINARY_DIR}/bin")
set(LIBRARY_OUTPUT_PATH "${PROJECT_BINARY_DIR}/lib")

include_directories("${PROJECT_BINARY_DIR}/3rd")
include_directories("${PROJECT_BINARY_DIR}/../CoSocket")
include_directories("${PROJECT_BINARY_DIR}/../CoSocket/CoSocket/net")
include_directories("${PROJECT_BINARY_DIR}/../CoSocket/CoSocket/base")
include_directories("${PROJECT_BINARY_DIR}/../CoSocket/CoSocket/3rd")

add_library(amfx
    ${PROJECT_BINARY_DIR}/3rd/libamfx/amf.cc
    ${PROJECT_BINARY_DIR}/3rd/libamfx/utils.cc
    )

add_executable(RtmpHeaderTest
    src/RtmpHeaderTest.cpp
    src/ByteStream.cpp
    src/RtmpHeader.cpp
    )

add_executable(StreamProcessTest
    src/StreamProcessTest.cpp
    src/StreamProcess.cpp
    src/ByteStream.cpp
    src/RtmpHeader.cpp
    )

target_link_libraries(StreamProcessTest
    amfx
    )

add_executable(RtmpServer
    src/Main.cpp
    src/RtmpServer.cpp
    src/StreamProcess.cpp
    src/ByteStream.cpp
    src/RtmpHeader.cpp
    src/DataCache.cpp
    )

target_link_libraries(RtmpServer
    CoSocket
    amfx
    pthread
    )
