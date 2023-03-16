
# Set the minimum version of CMake that can be used
# To find the cmake version run
# $ cmake --version
cmake_minimum_required(VERSION 3.16.3)

# Set the project name
project (example_glfw_opengl2_cmake LANGUAGES C CXX)

set(CMAKE_SYSTEM_NAME Linux)

#set(CMAKE_C_COMPILER aarch64-linux-gnu-gcc)
#set(CMAKE_CXX_COMPILER aarch64-linux-gnu-g++)
#set(CMAKE_FIND_ROOT_PATH /usr/bin)
#set(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)
#set(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)

#set(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)

set(SRC_DIR "${CMAKE_CURRENT_SOURCE_DIR}")

set(IMGUI_DIR "${SRC_DIR}/../..")
set(GLAD_DIR "${SRC_DIR}/../../../glad")

#The simplest solution may be to add HINTS to each find_* request.
find_library(glad NAMES libglad lglad libglad.a HINTS ${GLAD_DIR})

# check if boost was found
#if(glad_FOUND)
#    message ("glad found")
#else()
#    message (FATAL_ERROR "Cannot find glad")
#endif()
message("    -------     ")
message(${glad})

message("    -------     ")



## Find dependencies
#find_package(glfw3 REQUIRED)
#find_library(GL REQUIRED)
#find_library(glut REQUIRED)
find_package(X11 REQUIRED)
find_package(Threads REQUIRED)

add_library(IMGUI STATIC)

target_sources( IMGUI
                PRIVATE
                    ${IMGUI_DIR}/imgui_demo.cpp
                    ${IMGUI_DIR}/imgui_draw.cpp
                    ${IMGUI_DIR}/imgui_tables.cpp
                    ${IMGUI_DIR}/imgui_widgets.cpp
                    ${IMGUI_DIR}/imgui.cpp

                PRIVATE
                    ${IMGUI_DIR}/backends/imgui_impl_opengl2.cpp
                    ${IMGUI_DIR}/backends/imgui_impl_glfw.cpp
                )

target_include_directories( IMGUI
                            PUBLIC ${IMGUI_DIR}
                            PUBLIC ${IMGUI_DIR}/backends                            
                            "${PROGRAMS-PREFIX}/glad/include"
                            )

## Create main executable
add_executable(${PROJECT_NAME})
target_sources(${PROJECT_NAME} PUBLIC main.cpp)
#target_link_libraries(${PROJECT_NAME} IMGUI glfw3 GL glut ${CMAKE_THREAD_LIBS_INIT} ${X11_LIBRARIES} ${CMAKE_DL_LIBS} ${glad})
target_link_libraries(${PROJECT_NAME} IMGUI glfw GL glut ${CMAKE_THREAD_LIBS_INIT} ${X11_LIBRARIES} ${CMAKE_DL_LIBS} ${glad})
set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})



