cmake_minimum_required(VERSION 3.18)

project(smallGPU LANGUAGES C)

set(CMAKE_C_STANDARD 11)
set(CMAKE_C_STANDARD_REQUIRED ON)

find_package(OpenMP)

set(OpenGL_GL_PREFERENCE "GLVND")
find_package(OpenGL REQUIRED)
message(STATUS "OPENGL_gl_LIBRARY: ${OPENGL_gl_LIBRARY}")

if(MSVC)
    set(GLUT_ROOT_PATH "${CMAKE_SOURCE_DIR}/freeglut")
    message(STATUS "GLUT_ROOT_PATH: ${GLUT_ROOT_PATH}")
endif()
find_package(GLUT REQUIRED)
message(STATUS "GLUT_FOUND: ${GLUT_FOUND}")
message(STATUS "GLUT_INCLUDE_DIR: ${GLUT_INCLUDE_DIR}")
message(STATUS "GLUT_LIBRARIES: ${GLUT_LIBRARIES}")

if(APPLE)
    add_definitions(-Wno-deprecated-declarations)
endif()

find_package(OpenCL REQUIRED)



add_executable(smallptCPU smallptCPU.c displayfunc.c)

target_link_libraries(smallptCPU PUBLIC OpenGL::GL OpenGL::GLU GLUT::GLUT m)

if(OpenMP_FOUND)
    target_link_libraries(smallptCPU PUBLIC OpenMP::OpenMP_C)
endif()

add_executable(smallptGPU smallptGPU.c displayfunc.c)
target_link_libraries(smallptGPU OpenGL::GL OpenGL::GLU GLUT::GLUT m OpenCL::OpenCL)