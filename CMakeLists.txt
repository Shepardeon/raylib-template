cmake_minimum_required(VERSION 3.24..3.30)
project(Game VERSION 1.0.0 LANGUAGES C CXX)

# Set wether it's a debug or release build
set(CMAKE_BUILD_TYPE Debug)

include(FetchContent)

# Generate compile_command.json
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Fetch raylib
set(RAYLIB_VERSION 5.5)

FetchContent_Declare(
  raylib
  DOWNLOAD_EXTRACT_TIMESTAMP OFF
  URL https://github.com/raysan5/raylib/archive/refs/tags/${RAYLIB_VERSION}.tar.gz
  FIND_PACKAGE_ARGS
)

FetchContent_MakeAvailable(raylib)

# Raylib options
set(BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
set(BUILD_GAMES OFF CACHE BOOL "" FORCE)
set(GRAPHICS GRAPHICS_API_OPENGL_33 CACHE STRING "" FORCE)
set(PLATFORM Desktop CACHE STRING "" FORCE)

# Current project
add_executable(${PROJECT_NAME})

file(GLOB_RECURSE MY_SOURCES CONFIGURE_DEPENDS "${CMAKE_CURRENT_SOURCE_DIR}/src/*.cpp")
file(COPY "${CMAKE_CURRENT_SOURCE_DIR}/assets" DESTINATION "${CMAKE_CURRENT_SOURCE_DIR}/bin")

set_target_properties(${PROJECT_NAME} PROPERTIES RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin)

target_sources("${CMAKE_PROJECT_NAME}" PRIVATE ${MY_SOURCES})
target_include_directories("${CMAKE_PROJECT_NAME}" PRIVATE ${CMAKE_CURRENT_SOURCE_DIR}/include)

target_link_options("${CMAKE_PROJECT_NAME}" PRIVATE -static-libgcc -static-libstdc++)
target_link_libraries("${CMAKE_PROJECT_NAME}" PRIVATE raylib)