cmake_minimum_required(VERSION 3.5)
set(CMAKE_C_STANDARD "11")
set(CMAKE_CXX_STANDARD "11")
set(CMAKE_CXX_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -Wall -g")
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -Werror")
set(CMAKE_C_FLAGS_DEBUG "${CMAKE_C_FLAGS_DEBUG} -Wall -g")
set(CMAKE_C_FLAGS_RELEASE "${CMAKE_C_FLAGS_RELEASE} -Werror")
set(CMAKE_INSTALL_PREFIX ${CMAKE_CURRENT_LIST_DIR}/install)
set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_LIST_DIR}/cmake)

project(wvmcc)
set(PROJECT_SOURCE_DIR ${CMAKE_CURRENT_LIST_DIR}/src)
set(PROJECT_TEST_DIR ${CMAKE_CURRENT_LIST_DIR}/test)

# version
file(READ ${CMAKE_CURRENT_LIST_DIR}/VERSION WVMCC_VERSION)
add_definitions("-DWVMCC_VERSION=\"${WVMCC_VERSION}\"")

# src
add_subdirectory(src)

# test
if(CMAKE_BUILD_TYPE STREQUAL "Debug")
    option(ENABLE_TESTS "Enable test" ON)
    if(ENABLE_TESTS)
        include(CTest)
        find_package(SkyPat REQUIRED)
        include_directories(${SKYPAT_INCLUDE_DIR})
        link_directories(${SKYPAT_LIBRARY_DIR})
        add_subdirectory(test)
    endif(ENABLE_TESTS)
endif(CMAKE_BUILD_TYPE STREQUAL "Debug")