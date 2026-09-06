cmake_minimum_required(VERSION 3.22)

project(ZabrakDevaronianHair LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(RE_UE4SS_SOURCE_DIR "" CACHE PATH "Path to a pinned RE-UE4SS source checkout")

if(NOT EXISTS "${RE_UE4SS_SOURCE_DIR}/CMakeLists.txt")
    message(FATAL_ERROR "Set RE_UE4SS_SOURCE_DIR to a recursive RE-UE4SS checkout")
endif()

add_subdirectory(
    "${RE_UE4SS_SOURCE_DIR}"
    "${CMAKE_CURRENT_BINARY_DIR}/re-ue4ss"
)

add_library(ZabrakDevaronianHair SHARED
    src/dllmain.cpp
)

target_compile_features(
    ZabrakDevaronianHair
    PRIVATE
    cxx_std_23
)

target_link_libraries(
    ZabrakDevaronianHair
    PUBLIC
    UE4SS
    PolyHook_2
)

if(MSVC)
    target_link_options(
        ZabrakDevaronianHair
        PRIVATE
        "/PDBALTPATH:main.pdb"
    )
endif()

set_target_properties(
    ZabrakDevaronianHair
    PROPERTIES
    OUTPUT_NAME "main"
)
