cmake_minimum_required(VERSION 3.20)
project(gpt2_engine CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

add_compile_options(-Wall -Wextra -Wpedantic)

add_executable(engine src/main.cpp)
target_include_directories(engine PRIVATE include)

enable_testing()
add_executable(test_placeholder tests/test_placeholder.cpp)
target_include_directories(test_placeholder PRIVATE include)
add_test(NAME placeholder COMMAND test_placeholder)
