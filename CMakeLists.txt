cmake_minimum_required(VERSION 3.20)

project(wingui
    VERSION 0.1.0
    LANGUAGES C
)

add_library(wingui STATIC)

target_sources(wingui PRIVATE
    src/window.c
)

target_include_directories(wingui
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src>
        $<INSTALL_INTERFACE:include>
)

target_link_libraries(wingui
    PUBLIC
        user32
        gdi32
)

target_compile_definitions(wingui
    PRIVATE
        WIN32_LEAN_AND_MEAN
        NOMINMAX
)

if (MSVC)
    target_compile_options(wingui PRIVATE
        /W4
        /permissive-
    )
else()
    target_compile_options(wingui PRIVATE
        -Wall
        -Wextra
        -Wpedantic
    )
endif()
