# verificamos que la version de cmake sea por lo menos 3.25
cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

# Define your project

project(
    "FirstProjectInCMake"
    VERSION 1.0.0
   DESCRIPTION "CMake C Project Template for Operating Systems II"
    LANGUAGES C
)

# Define the C standard

set(CMAKE_C_STANDARD 17)

# Include the 'include' directory where the headers are located

include_directories(include)

# add src directory to the project
file(GLOB_RECURSE SOURCES "src/*.c")

# Create the executable
add_executable(${PROJECT_NAME} ${SOURCES})

