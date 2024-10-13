cmake_minimum_required(VERSION 3.15)

project(imnodes)

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED True)

set(CMAKE_WINDOWS_EXPORT_ALL_SYMBOLS ON)

# determine whether this is a standalone project or included by other projects
if (NOT DEFINED IMNODES_STANDALONE_PROJECT)
    if (CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
        set(IMNODES_STANDALONE_PROJECT ON)
    else()
        set(IMNODES_STANDALONE_PROJECT OFF)
    endif ()
endif()

# cmake options
option(IMNODES_EXAMPLES "Build examples" ${IMNODES_STANDALONE_PROJECT})


if(MSVC)
    add_compile_definitions(SDL_MAIN_HANDLED)
    add_compile_options(/WX)
    # replace existing /W to avoid warning
    if(CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
        string(REGEX REPLACE "/W[0-4]" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
    else()
        add_compile_options(/W4)
    endif()
else()
    add_compile_options(-Wall -Wextra -Wpedantic -Werror)
endif()

# Imnodes

if (NOT DEFINED SAGE_DIR)
    message(FATAL_ERROR "SAGE_DIR has not been set!")
endif()

include_directories(${SAGE_DIR}/library/imgui)
add_library(imnodes STATIC
    imnodes.h
    imnodes_internal.h
    imnodes.cpp)
target_link_libraries(imnodes PUBLIC imgui)