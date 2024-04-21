cmake_minimum_required(VERSION 3.16)

# set the project name
project(tremor C)

option(TREMOR_LOW_ACCURACY "enable 32 bit only multiply operations" OFF)
option(TREMOR_LESS_THAN_64K "limit playback ability to <64kHz files" OFF)

set(SRC_LIST_C
    "bitwise.c"
    "codebook.c"
    "dsp.c"
    "floor0.c"
    "floor1.c"
    "floor_lookup.c"
    "framing.c"
    "info.c"
    "mapping0.c"
    "mdct.c"
    "misc.c"
    "res012.c"
    "vorbisfile.c"
)

if(TREMOR_LOW_ACCURACY)
    add_compile_definitions(_LOW_ACCURACY_)
endif()

if(TREMOR_LESS_THAN_64K)
    add_compile_definitions(LIMIT_TO_64kHz)
endif()

# define libraries
add_library(tremor ${SRC_LIST_C})

# define location for header files
target_include_directories(tremor PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})
target_link_libraries(tremor PUBLIC ogg)
target_compile_options(tremor PRIVATE 
    "-Wno-char-subscripts"
    "-Wno-misleading-indentation"
    "-Wno-maybe-uninitialized"
    "-Wno-type-limits"
    "-Wno-unused-label"
)
