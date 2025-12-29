cmake_minimum_required(VERSION 3.15)

project(wingui
    VERSION 0.1.0
    LANGUAGES C
)

option(WINGUI_BUILD_EXAMPLES "Build examples" ON)
option(WINGUI_BUILD_SHARED "Build shared library" OFF)

# Library type
if (WINGUI_BUILD_SHARED)
    add_library(wingui SHARED)
else()
    add_library(wingui STATIC)
endif()

target_sources(wingui PRIVATE
    src/window.c
)

# Public headers
target_include_directories(wingui
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

# Windows system libs (MSVC + MinGW compatible)
target_link_libraries(wingui
    PUBLIC
        user32
        gdi32
)

# Compile definitions
target_compile_definitions(wingui
    PRIVATE
        WIN32_LEAN_AND_MEAN
        NOMINMAX
)

# Compiler warnings
if (MSVC)
    target_compile_options(wingui PRIVATE /W4)
else()
    target_compile_options(wingui PRIVATE -Wall -Wextra -Wpedantic)
endif()

# Examples
if (WINGUI_BUILD_EXAMPLES)
    add_executable(basic_window examples/basic_window.c)
    target_link_libraries(basic_window wingui)
endif()
