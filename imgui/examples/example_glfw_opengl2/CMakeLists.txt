
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
#set(GLAD_DIR "${SRC_DIR}/../../../glad/build_${CMAKE_SYSTEM_PROCESSOR}")
set(GLAD_DIR "${SRC_DIR}/../../../glad")

#The simplest solution may be to add HINTS to each find_* request.
#find_library(glad NAMES libglad lglad libglad.a HINTS ${GLAD_DIR})

# check if boost was found
#if(glad_FOUND)
#	message ("glad found in ${GLAD_DIR}")
#else()
#	message ("Cannot find glad in ${GLAD_DIR}")
#	set(glad ${GLAD_DIR}/libglad.a)
#endif()
#message("    -------     ")
#message(${glad})
# message(${CMAKE_CXX_FLAGS})
#message("    -------     ")



## Find dependencies
find_package(glfw3 REQUIRED)
# find_library(GL REQUIRED)
# find_library(glut REQUIRED)
# find_package(X11 REQUIRED)
find_package(Threads REQUIRED)

add_library(GLAD STATIC)

target_sources( GLAD
                PRIVATE
                    ${GLAD_DIR}/src/glad.c
                )

target_include_directories( GLAD
                            PUBLIC ${GLAD_DIR}/include
                            )

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
#                            "${PROGRAMS-PREFIX}/glad/include"
                            )

## Create main executable
add_executable(${PROJECT_NAME})
target_sources(${PROJECT_NAME} PUBLIC main.cpp)
#target_link_libraries(${PROJECT_NAME} IMGUI glfw3 GL glut ${CMAKE_THREAD_LIBS_INIT} ${X11_LIBRARIES} ${CMAKE_DL_LIBS} ${glad})
#target_link_libraries(${PROJECT_NAME} IMGUI GLAD glfw GL glut ${CMAKE_THREAD_LIBS_INIT} ${X11_LIBRARIES} ${CMAKE_DL_LIBS})
# target_link_libraries(${PROJECT_NAME} IMGUI GLAD glfw GL glut ${CMAKE_THREAD_LIBS_INIT} ${CMAKE_DL_LIBS})
# try to put x11 before glfw3 !

#target_link_libraries(${PROJECT_NAME} IMGUI glfw3 GL ${X11_LIBRARIES} ${CMAKE_THREAD_LIBS_INIT} ${CMAKE_DL_LIBS})
#target_link_libraries(${PROJECT_NAME} X11 IMGUI glfw3 GL ${CMAKE_THREAD_LIBS_INIT} ${CMAKE_DL_LIBS})
#target_link_libraries(${PROJECT_NAME} IMGUI glfw3 GL ${X11_LIBRARIES} X11 ${CMAKE_THREAD_LIBS_INIT} ${CMAKE_DL_LIBS})
target_link_libraries(${PROJECT_NAME} IMGUI glfw3 GL X11)
# ${CMAKE_THREAD_LIBS_INIT} ${CMAKE_DL_LIBS})
set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})


# install(TARGETS ${PROJECT_NAME} DESTINATION /home/kalo/work/install/${CMAKE_SYSTEM_PROCESSOR}/bin)
# install(TARGETS ${PROJECT_NAME} DESTINATION ${CMAKE_INSTALL_PREFIX}/usr/bin)
install(TARGETS ${PROJECT_NAME} DESTINATION bin)
