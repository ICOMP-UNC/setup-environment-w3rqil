# verificamos que la version de cmake sea por lo menos 3.25
cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

# Define your project

project(
    "FirstProjectInCMake"
    VERSION 1.0.0
   DESCRIPTION "CMake C Project Template for Operating Systems II"
    LANGUAGES C
)
# Enable testing
include(CTest)

# Define the C standard

set(CMAKE_C_STANDARD 17)

# Include the 'include' directory where the headers are located

include_directories(include)

#to automatic download external deps, we need to use FetchContent
# see: https://cmake.org/cmake/help/latest/module/FetchContent.html
include(FetchContent)

# In this example we are gonna use unity to download and test our program
set(UNITY_GIT_URL "https://github.com/ThrowTheSwitch/Unity.git")

# Enable debug
set(FETCHCONTEN_QUIET OFF)

FetchContent_Declare(
    Unity
    GIT_REPOSITORY ${UNITY_GIT_URL}
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/external/Unity
    UPDATE_COMMAND ""
)
# insures the named dependencies have been populated
FetchContent_MakeAvailable(Unity)


# add src directory to the project
file(GLOB_RECURSE SOURCES "src/*.c")

# Create the executable
add_executable(${PROJECT_NAME} ${SOURCES})

#compile labraries
add_subdirectory(lib/libdyn)
add_subdirectory(lib/libsta)

#include headers in lib
# See: https://cmake.org/cmake/help/latest/command/target_include_directories.html
target_include_directories(${PROJECT_NAME} PUBLIC lib/libdyn/include)
target_include_directories(${PROJECT_NAME} PUBLIC lib/libsta/include)

# Link the libraries
target_link_libraries(${PROJECT_NAME} StatiCLib DynamiCLib)

# add test's subdirectory
if (RUN_TESTS EQUAL 1 OR RUN_COVERAGE EQUAL 1)
  add_subdirectory(tests)
endif()
