cmake_minimum_required(VERSION 3.21)
project(StepUpOntoSKSE VERSION 1.5.1 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Build against the same CommonLibSSE-NG the NPCPathingNG plugin uses.
list(APPEND CMAKE_PREFIX_PATH
    "${CMAKE_CURRENT_SOURCE_DIR}/../Modern NPC Pathing/extern/CommonLibSSE/install"
    "${CMAKE_CURRENT_SOURCE_DIR}/../Modern NPC Pathing/extern/CommonLibSSE/build/vcpkg_installed/x64-windows-static"
)
find_package(CommonLibSSE CONFIG REQUIRED)

add_library(${PROJECT_NAME} SHARED
    src/main.cpp
    src/Settings.cpp
    src/Menu.cpp
    src/Hooks.cpp
    src/Raycast.cpp
    src/StepUpManager.cpp
)

target_precompile_headers(${PROJECT_NAME} PRIVATE src/PCH.h)
target_link_libraries(${PROJECT_NAME} PRIVATE CommonLibSSE::CommonLibSSE)

set_property(TARGET ${PROJECT_NAME} PROPERTY
    MSVC_RUNTIME_LIBRARY "MultiThreaded$<$<CONFIG:Debug>:Debug>"
)

target_include_directories(${PROJECT_NAME} PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/src)

target_compile_definitions(${PROJECT_NAME} PRIVATE _UNICODE UNICODE)

set(OUTPUT_DIR "${CMAKE_CURRENT_SOURCE_DIR}/package/SKSE/Plugins")
set_target_properties(${PROJECT_NAME} PROPERTIES
    OUTPUT_NAME "StepUpOntoSKSE"
    SUFFIX ".dll"
    RUNTIME_OUTPUT_DIRECTORY "${OUTPUT_DIR}"
    RUNTIME_OUTPUT_DIRECTORY_RELEASE "${OUTPUT_DIR}"
)
