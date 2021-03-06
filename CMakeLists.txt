
SET(CMAKE_MINIMUM_REQUIRED_VERSION 3.12)

cmake_minimum_required(VERSION ${CMAKE_MINIMUM_REQUIRED_VERSION})

SET(VCPKG_ROOT $ENV{VCPKG_ROOT} CACHE STRING "VCPKG_ROOT")
if(NOT DEFINED VCPKG_ROOT)
    message(FATAL_ERROR "VCPKG_ROOT is not set!")
endif()

if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
    set(CMAKE_TOOLCHAIN_FILE "${VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake"
            CACHE STRING "")
endif()

project(FMICPP)

set(CMAKE_CXX_STANDARD 17)
list(APPEND CMAKE_MODULE_PATH "${CMAKE_SOURCE_DIR}/cmake")

SET(BOOST_COMPONENTS system filesystem)
if (BUILD_TESTS)
    SET(BOOST_COMPONENTS ${BOOST_COMPONENTS} unit_test_framework)
endif()
find_package(Boost 1.67 COMPONENTS ${BOOST_COMPONENTS} REQUIRED)
include_directories(${Boost_INCLUDE_DIRS})

find_package(Libzip REQUIRED)

option(BUILD_TESTS "Build tests" OFF)
option(BUILD_EXAMPLES "Build examples" ON)

add_subdirectory(src)

if (BUILD_TESTS)
    add_subdirectory(tests)
endif()

if (BUILD_EXAMPLES)
    add_subdirectory(examples)
endif()