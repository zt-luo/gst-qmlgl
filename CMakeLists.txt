cmake_minimum_required(VERSION 3.5)

project(gstqmlgl LANGUAGES CXX)


set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_AUTOMOC ON)

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

add_library(${PROJECT_NAME} SHARED ${src_list})
add_library(${PROJECT_NAME}_static STATIC ${src_list})
target_compile_definitions(${PROJECT_NAME}_static PRIVATE STATIC_QMLGL_PLUGIN)
