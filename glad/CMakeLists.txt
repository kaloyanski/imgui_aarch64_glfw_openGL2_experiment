
# Set the minimum version of CMake that can be used
# To find the cmake version run
# $ cmake --version
cmake_minimum_required(VERSION 3.16.3)

# Set the project name
project (glad LANGUAGES C CXX)

set(SRC_DIR "${CMAKE_CURRENT_SOURCE_DIR}/src")
set(INC_DIR "${CMAKE_CURRENT_SOURCE_DIR}/include")

set (PROGRAMS-PREFIX "~/programs")

add_library(glad STATIC ${SRC_DIR}/glad.c)

    
target_include_directories( glad
                            PUBLIC
                            ${INC_DIR}
                            )
                            
## Find dependencies




