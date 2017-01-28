cmake_minimum_required(VERSION 3.3)
project(opengl_new)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -std=c++11")
set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/build)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR})

set(GLFW_SOURCE_DIR "external/glfw")
set(GLM_SOURCE_DIR "external/glm")
set(GLEW_SOURCE_DIR "external/glew")


find_package(OpenGL REQUIRED)

if (MINGW)
    SET(CMAKE_EXE_LINKER_FLAGS "-static")
endif (MINGW)

# Building only the GLFW lib
set(BUILD_SHARED_LIBS OFF CACHE BOOL "")
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "")
set(GLFW_BUILD_TESTS OFF CACHE BOOL "")
set(GLFW_BUILD_DOCS OFF CACHE BOOL "")
set(GLFW_INSTALL OFF CACHE BOOL "")

add_subdirectory(${GLFW_SOURCE_DIR} ${GLEW_SOURCE_DIR})

include_directories(${GLFW_SOURCE_DIR}/include
        ${GLFW_SOURCE_DIR}/deps
        ${GLM_SOURCE_DIR}
        ${GLEW_SOURCE_DIR}/include)
include_directories(${OPENGL_INCLUDE_DIRS})

add_definitions(
        -DTW_STATIC
        -DGLEW_STATIC
)

add_library(glew STATIC
        ${GLEW_SOURCE_DIR}/src/glew.c
        ${GLEW_SOURCE_DIR}/include
        )

target_link_libraries(glew ${OPENGL_LIBRARIES})


add_executable(opengl_new main.cpp )
target_link_libraries("opengl_new" glfw ${OPENGL_LIBRARIES} ${GLFW_LIBRARIES} glew)