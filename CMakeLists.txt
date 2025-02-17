# =========================================================================== #
# @file CMakeLists.txt
# @brief CMake configuration file for Slumber.
#
# This file sets up the Slumber project, defines its version, configures 
# dependencies, and compiles the source code.
# =========================================================================== #

cmake_minimum_required(VERSION 3.30.0)

# --------------------------------------------------------------------------- #
# @section Project Configuration
# Defines the project name, version, and description.
# --------------------------------------------------------------------------- #
project(
    slumber
    VERSION 0.1.0
    DESCRIPTION "A lightweight and efficient API testing tool, designed to give you restful nights."
    LANGUAGES CXX
)

# --------------------------------------------------------------------------- #
# @section Versioning
# Defines the major, minor, and patch versions for Slumber.
# --------------------------------------------------------------------------- #
set(SLUMBER_VERSION_MAJOR 1)
set(SLUMBER_VERSION_MINOR 0)
set(SLUMBER_VERSION_PATCH 0)
set(SLUMBER_VERSION_STRING "${SLUMBER_VERSION_MAJOR}.${SLUMBER_VERSION_MINOR}.${SLUMBER_VERSION_PATCH}")
set(SLUMBER_PROJECT_NAME "Slumber")

# --------------------------------------------------------------------------- #
# @section Compiler Settings
# Enforces C++17 as the standard and ensures it is required.
# --------------------------------------------------------------------------- #
set(EXECUTABLE_NAME "slumber")
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# --------------------------------------------------------------------------- #
# @section Include Directories
# Adds necessary include directories for both generated and source files.
# --------------------------------------------------------------------------- #
include_directories(
    ${CMAKE_BINARY_DIR}/include
    ${CMAKE_SOURCE_DIR}/include
    ${CMAKE_SOURCE_DIR}/source/include
    ${CMAKE_SOURCE_DIR}/frontend/include
)

# --------------------------------------------------------------------------- #
# @section Configuration Files
# Processes the slumber.hpp.in template to generate slumber.hpp with version info.
# --------------------------------------------------------------------------- #
configure_file(include/slumber.hpp.in ${CMAKE_BINARY_DIR}/include/slumber.hpp @ONLY)

# --------------------------------------------------------------------------- #
# @section Source Files
# Collects all .cpp source files from the project recursively.
# --------------------------------------------------------------------------- #
file(GLOB_RECURSE SOURCES
    "source/*.cpp"
    "frontend/*.cpp"
)

# --------------------------------------------------------------------------- #
# @section Executable
# Defines the main executable for Slumber.
# --------------------------------------------------------------------------- #
add_executable(${EXECUTABLE_NAME} ${SOURCES})

# --------------------------------------------------------------------------- #
# @section Dependencies
# Finds and links the GTKMM library.
# --------------------------------------------------------------------------- #
find_package(PkgConfig REQUIRED)
pkg_check_modules(GTKMM REQUIRED gtkmm-3.0)

# Adds include and library directories for GTKMM.
include_directories(${GTKMM_INCLUDE_DIRS})
link_directories(${GTKMM_LIBRARY_DIRS})
add_definitions(${GTKMM_CFLAGS_OTHER})

# Links the GTKMM libraries to the Slumber executable.
target_link_libraries(${EXECUTABLE_NAME} ${GTKMM_LIBRARIES})
