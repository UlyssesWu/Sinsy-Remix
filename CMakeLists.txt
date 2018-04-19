cmake_minimum_required(VERSION 2.8)
project(SinsyRemix)

OPTION(build_ios "build ios library" FALSE)

ADD_DEFINITIONS(-DLOGGING_LEVEL=LL_ERROR)
#set(CMAKE_BUILD_TYPE Release)
#message("CMAKE_CURRENT_SOURCE_DIR = ${CMAKE_CURRENT_SOURCE_DIR}")
#message("CMAKE_CURRENT_BINARY_DIR = ${CMAKE_CURRENT_BINARY_DIR}")
#message("CMAKE_BINARY_DIR = ${CMAKE_BINARY_DIR}")

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

INCLUDE_DIRECTORIES(${PROJECT_SOURCE_DIR}/lib
        ${PROJECT_SOURCE_DIR}/lib/util
        ${PROJECT_SOURCE_DIR}/lib/converter
        ${PROJECT_SOURCE_DIR}/lib/hts_engine_API
        ${PROJECT_SOURCE_DIR}/lib/label
        ${PROJECT_SOURCE_DIR}/lib/score
        ${PROJECT_SOURCE_DIR}/lib/temporary
        ${PROJECT_SOURCE_DIR}/lib/xml
        ${PROJECT_SOURCE_DIR}/lib/chinese
        ${PROJECT_SOURCE_DIR}/lib/japanese
        ${PROJECT_SOURCE_DIR}/lib/hts_engine
        ${PROJECT_SOURCE_DIR}/include
        )

link_directories(/usr/local)
if (build_ios)
    set(build_demo_module false)
    set(build_gtest_module false)
    set(build_one_lib true)
    ADD_DEFINITIONS(-DCPPMARY_USE_IOSBLAS)
else()
    set(build_demo_module true)
    set(build_gtest_module false)
    set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
    set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/lib)
    set(cppmary_use_open_blas true)
endif()

if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    message("cppmary build: Debug")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O0 -g")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O0 -std=c++11")
elseif(CMAKE_BUILD_TYPE STREQUAL "RelWithDebInfo")
    message("cppmary build: RelwithDebInfo")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3 -g")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -std=c++11")
else()
    message("cppmary build: Release")
    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O3")
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -std=c++11")
endif()

ADD_SUBDIRECTORY(lib)
if (build_demo_module)
    ADD_SUBDIRECTORY(bin)
endif()

set(CMAKE_INSTALL_PREFIX ${CMAKE_BINARY_DIR}/release/)

install(DIRECTORY include/ DESTINATION include
        FILES_MATCHING PATTERN "*.h*")
install(DIRECTORY 3rdparty DESTINATION include
        FILES_MATCHING PATTERN "*.h*")

file(GLOB files "data/*")
foreach(file ${files})
    if (MSVC)
        file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/release/data)
        file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/release/data)
    else(MSVC)
        file(COPY ${file} DESTINATION ${CMAKE_BINARY_DIR}/release/data)
    endif(MSVC)
endforeach()
