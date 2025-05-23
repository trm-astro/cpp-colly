cmake_minimum_required(VERSION 3.23)
project(cpp-colly)

include(ExternalProject)

message("\nCMakeLists.txt for cpp-colly")
message("PROJECT_SOURCE_DIR: ${PROJECT_SOURCE_DIR}")
message("CMAKE_BINARY_DIR: ${CMAKE_BINARY_DIR}")

set(PACKAGE_NAME colly)

############################################################
# Define sources
############################################################

set(SOURCES
    src/write_molly_head.cc
    src/read_molly_head.cc
    src/find_groups.cc
    src/rejig_molly_head.cc
    src/skip_molly_data.cc
    src/skip_bytes.cc
)

############################################################
# Define the library
############################################################
add_library(${PACKAGE_NAME} SHARED 
    ${SOURCES}
)

target_include_directories(${PACKAGE_NAME}
    PUBLIC
        $<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/include>
        $<INSTALL_INTERFACE:include>
)

############################################################
# Check for and install dependencies
############################################################

# Configure the PLPLOT library
if(NOT DEFINED CMAKE_COMMON)
    set(CMAKE_COMMON ${PROJECT_SOURCE_DIR}/../../cmake_common)
endif()
if(NOT DEFINED PLPLOT_FOUND)
    include("${CMAKE_COMMON}/plplot_install.cmake")
endif()

# find_package(SUBS REQUIRED)

# if (NOT SUBS_FOUND)
#     message(FATAL_ERROR "SUBS library not found.")
# endif()

target_include_directories(${PACKAGE_NAME}
    PUBLIC 
        $<BUILD_INTERFACE:${PLPLOT_INCLUDE_DIR}>
)
target_link_libraries(${PACKAGE_NAME} 
    PUBLIC 
        subs
)

# Set RPATH properties
# set_target_properties(${PACKAGE_NAME} PROPERTIES
#     INSTALL_RPATH "${CMAKE_INSTALL_PREFIX}/lib"
#     BUILD_WITH_INSTALL_RPATH TRUE
#     INSTALL_RPATH_USE_LINK_PATH TRUE
#     BUILD_RPATH_USE_ORIGIN TRUE
# )

############################################################
# Create an executable
############################################################

# No executable in this code, this is a library

############################################################
# Install
############################################################
include("${CMAKE_COMMON}/install_lib.cmake") # Get the generic install function

install_lib(${PACKAGE_NAME})

message("cpp-colly CMakeLists.txt complete\n")