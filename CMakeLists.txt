cmake_minimum_required(VERSION 3.5)

project(gstqmlgl LANGUAGES CXX)

set(CMAKE_AUTOMOC ON)

set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)


find_package(Qt5 COMPONENTS Qml Quick Gui REQUIRED)

if (${Qt5_FOUND})
    message(STATUS "Found Qt " ${Qt5_VERSION})
else()
    message(FATAL_ERROR  "Couldn't find Qt")
endif()

link_libraries(
    Qt5::Qml
    Qt5::Quick
    Qt5::Gui
    )

find_package(OpenGL REQUIRED)

if (NOT ${OpenGL_FOUND})
    message(FATAL_ERROR  "Couldn't find OpenGL")
endif()

link_libraries(${OPENGL_LIBRARIES})


find_package(PkgConfig REQUIRED)
pkg_search_module(GST REQUIRED gstreamer-1.0)
pkg_search_module(GST_VIDEO REQUIRED gstreamer-video-1.0)
pkg_search_module(GST_GL REQUIRED gstreamer-gl-1.0)

include_directories(
    ${GST_INCLUDE_DIRS} ${GST_VIDEO_INCLUDE_DIRS} ${GST_GL_INCLUDE_DIRS}
    )

link_libraries(
    ${GST_LDFLAGS} ${GST_VIDEO_LDFLAGS} ${GST_GL_LDFLAGS}
    )

set(src_list
    "gstqsgtexture.h"
    "gstqtgl.h"
    "gstqtglutility.h"
    "gstqtsink.h"
    "gstqtsrc.h"
    "qtwindow.h"
    "qtitem.h"

    "gstplugin.cc"
    "gstqtglutility.cc"
    "gstqsgtexture.cc"
    "gstqtsink.cc"
    "gstqtsrc.cc"
    "qtwindow.cc"
    "qtitem.cc"
    )

if (WIN32)
message(STATUS "target system: WIN32")
add_compile_definitions(HAVE_QT_WIN32)
elseif (APPLE)
add_compile_definitions(HAVE_QT_MAC)
message(STATUS "target system: APPLE")
elseif (ANDROID)
message(STATUS "target system: ANDROID")
add_compile_definitions(HAVE_QT_ANDROID)
endif()

if (MSVC)
message(STATUS "useing MSVC")
link_libraries(opengl32.lib)
endif()

add_library(${PROJECT_NAME} SHARED ${src_list})
add_library(${PROJECT_NAME}_static STATIC ${src_list})
