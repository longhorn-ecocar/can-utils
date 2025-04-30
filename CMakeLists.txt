cmake_minimum_required(VERSION 3.12)
project(can_utils LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(Boost REQUIRED)

add_library(can_utils_lib STATIC
    dbc/dbc_parser.cpp
    v2c/v2c_transcoder.cpp)

target_include_directories(can_utils_lib PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}
    ${Boost_INCLUDE_DIRS})

target_link_libraries(can_utils_lib PUBLIC ${Boost_LIBRARIES})

option(GENERATE_SHARED_LIB_UNITTESTS "Build tests" OFF)
if(GENERATE_SHARED_LIB_UNITTESTS)
    enable_testing()
    find_package(GTest REQUIRED)
    add_executable(can_utils_lib_tests example/example.cpp)
    target_link_libraries(can_utils_lib_tests PRIVATE can_utils_lib GTest::gtest_main)
    add_test(NAME can_utils_tests COMMAND can_utils_lib_tests)
endif()
