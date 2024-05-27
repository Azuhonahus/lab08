cmake_minimum_required(VERSION 3.29.3)

project(formatter_lib)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

add_library(formatter_lib STATIC ${CMAKE_CURRENT_SOURCE_DIR}/formatter.cpp)

include_directories(${CMAKE_CURRENT_SOURCE_DIR})
