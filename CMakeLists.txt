cmake_minimum_required(VERSION 3.12)
project(dsc VERSION 1.0 LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)

# ----------------------- #
# Basic setup for project #
include_directories(include)
# Automatic detection of source files
file(GLOB_RECURSE SOURCES "src/*.cpp")
file(GLOB_RECURSE HEADERS "include/*.h")
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
# ----------------------- #

# ----------------------- #
# MPI setup
find_package(MPI REQUIRED)
include_directories(${MPI_INCLUDE_PATH})
# add MPI_CXX_LIBRARIES to LIBS
list(APPEND LIBS ${MPI_CXX_LIBRARIES})
# ----------------------- #

# Executables from app directory
file(GLOB APP_SOURCES "app/*.cpp")
foreach(APP_SOURCE ${APP_SOURCES})
    get_filename_component(APP_NAME ${APP_SOURCE} NAME_WE)
    # append .x to APP_NAME
    set(APP_NAME "${APP_NAME}.x")
    add_executable(${APP_NAME} ${APP_SOURCE} ${SOURCES})
    target_link_libraries(${APP_NAME} PUBLIC ${LIBS})
endforeach()

# Test directory
enable_testing()
add_subdirectory(tests)
