cmake_minimum_required(VERSION 3.20)

project(owl-server)

option(OWLSRV_BUILD_TESTS "Build unit tests (default OFF)" OFF)

set(CMAKE_CXX_STANDARD 20)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include(CMake/conan.cmake)
conan_cmake_run(REQUIRES
    boost/1.78.0
    BASIC_SETUP CMAKE_TARGETS
    GENERATORS cmake_find_package cmake_paths
    BUILD missing
    OPTIONS
        boost:shared=False 
        boost:without_test=False 
        boost:without_program_options=False 
        boost:without_filesystem=False 
        boost:without_system=False 
        boost:without_exception=False
        boost:without_contract=False
)
conan_basic_setup(NO_OUTPUT_DIRS KEEP_RPATHS)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/mqtt_cpp)
include_directories(${CMAKE_CURRENT_SOURCE_DIR}/mqtt_cpp/include)

add_subdirectory(src)

if (OWLSRV_BUILD_TESTS)
    enable_testing()
    add_subdirectory(test)
endif()
