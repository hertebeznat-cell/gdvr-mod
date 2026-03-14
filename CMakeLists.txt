cmake_minimum_required(VERSION 3.16.0 FATAL_ERROR)

project(gdvr VERSION 1.0.0)

find_package(Geode REQUIRED)

# Fetch OpenXR
FetchContent_Declare(
    openxr
    GIT_REPOSITORY "https://github.com/KhronosGroup/OpenXR-SDK.git"
    GIT_TAG "release-1.0.34"
)
FetchContent_MakeAvailable(openxr)

# The project target
file(GLOB_RECURSE SOURCES "src/*.cpp")
setup_geode_mod(${PROJECT_NAME})

target_sources(${PROJECT_NAME} PRIVATE ${SOURCES})
target_link_libraries(${PROJECT_NAME} PRIVATE 
    Geode::Geode 
    openxr_loader
)

# On Android, OpenXR may need the EGL/GLES3 headers and linking
if (GEODE_TARGET_PLATFORM STREQUAL "Android" OR ANDROID)
    target_link_libraries(${PROJECT_NAME} PRIVATE EGL GLESv3)
endif()
