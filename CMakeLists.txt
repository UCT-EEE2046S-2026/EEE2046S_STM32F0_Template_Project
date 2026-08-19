cmake_minimum_required(VERSION 4.2)

project(MODIFY_PROJECT_NAME)

set(TARGET_EXECUTABLE ${PROJECT_NAME})

enable_language(C ASM)

set(CMAKE_C_STANDARD 17)
set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS ON)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Default to Debug Build if Preset not set
if(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Debug")
endif()

include(CMake/scripts.cmake)
include(CMake/mcu.cmake)

set(CROSSCOMPILE_C_OPTIONS
    -fdata-sections
    -ffunction-sections
    -fstack-usage
)

set(CROSSCOMPILE_OPTIMISATION_OPTIONS_DEBUG
    -Og
    -g3
)
set(CROSSCOMPILE_OPTIMISATION_OPTIONS_RELEASE
    -Os
)

set(CROSSCOMPILE_ASM_OPTIONS
    -MMD
    -MP
)

set(TOOLCHAIN_LINK_LIBRARIES "m")

add_compile_options(
    ${CPU_COMPILE_OPTIONS}
    ${CROSSCOMPILE_C_OPTIONS}
    "$<$<COMPILE_LANGUAGE:ASM>:${CROSSCOMPILE_ASM_OPTIONS}>"
    "$<$<CONFIG:Debug>:${CROSSCOMPILE_OPTIMISATION_OPTIONS_DEBUG}>"
	"$<$<CONFIG:Release>:${CROSSCOMPILE_OPTIMISATION_OPTIONS_RELEASE}>"
)

add_compile_definitions(
    $<$<CONFIG:Debug>:DEBUG>
)

add_link_options(
    ${CPU_COMPILE_OPTIONS}
    --specs=nano.specs
    -T${LINKER_SCRIPT}

    LINKER:--gc-sections
    LINKER:--print-memory-usage
)

set(APPLICATION_INCLUDE_DIRECTORIES
    ${CMAKE_SOURCE_DIR}/Inc
)

file(GLOB APPLICATION_SOURCES
    ${CMAKE_SOURCE_DIR}/Src/*.c
)

add_executable(${TARGET_EXECUTABLE}
    ${APPLICATION_SOURCES}
    ${STARTUP_SCRIPT}
)

target_include_directories(${TARGET_EXECUTABLE} PRIVATE
    ${APPLICATION_INCLUDE_DIRECTORIES}
)

target_compile_options(${TARGET_EXECUTABLE} PRIVATE
    -Wall
    -Wextra
    -Wpedantic
)

target_link_options(${TARGET_EXECUTABLE} PRIVATE
    LINKER:-Map=${TARGET_EXECUTABLE}.map,--cref
)
set_target_properties(${CMAKE_PROJECT_NAME} 
    PROPERTIES ADDITIONAL_CLEAN_FILES ${TARGET_EXECUTABLE}.map
)

