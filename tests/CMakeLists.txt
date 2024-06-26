cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

# include headers project
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/../include)

# Collect tests
file(GLOB TESTS_FILES ${CMAKE_CURRENT_SOURCE_DIR}/unit/*.c)
file(GLOB SRC_FILES ${CMAKE_CURRENT_SOURCE_DIR}/../src/logic.c)

# LINK WITH UNITY HEADER

target_link_directories(${PROJECT_NAME} PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/external/Unity/src)

#Coverage
if(RUN_COVERAGE)
    message("Coverage enabled")
    find_program(LCOV_PATH lcov)
        if(NOT LCOV_PATH)
            message(FATAL_ERROR "lcov not found!")
    endif()

    find_program(GCOV_PATH gcov)
        if(NOT GCOV_PATH)
            message(FATAL_ERROR "gcov not found!")
    endif()

    find_program(GENHTML_PATH genhtml)
        if(NOT GENHTML_PATH)
            message(FATAL_ERROR "genhtml not found!")
    endif()

    set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -g -fprofile-arcs -ftest-coverage --coverage")
    set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} -lgcov --coverage") 

endif() 

# create test
add_executable(test_${PROJECT_NAME} ${TESTS_FILES} ${SRC_FILES})

#link with unity
target_link_libraries(test_${PROJECT_NAME} unity)

# add test
# see https://cmake.org/cmake/help/latest/command/add_test.html
add_test(NAME test_${PROJECT_NAME} COMMAND test_${PROJECT_NAME})
