cmake_minimum_required(VERSION 3.1)
project(manifest VERSION 1.0.0 LANGUAGES CXX)

if(NOT WIN32)
    message(FATAL_ERROR "This bug only applies to Windows.")
endif()
if(NOT MSVC)
    message(WARNING "This bug has not been tested with MinGW, though I'm certain a manifest can be applied with it.")
endif()

set(FLTK_SKIP_OPENGL TRUE) # My compiled FTLK lacks GL, shouldn't matter.
find_package(FLTK)

configure_file(resource.rc.in "${CMAKE_CURRENT_BINARY_DIR}/resource.rc" @ONLY)

add_executable(manifest WIN32
    manifest.cpp "${CMAKE_CURRENT_BINARY_DIR}/resource.rc")
target_include_directories(manifest PRIVATE ${FLTK_INCLUDE_DIR})
target_link_libraries(manifest PRIVATE Gdiplus ${FLTK_LIBRARIES})
if(MSVC)
    # We supply our own manifest, don't use the default one.
    set_target_properties(manifest PROPERTIES LINK_FLAGS "/MANIFEST:NO")
endif()
