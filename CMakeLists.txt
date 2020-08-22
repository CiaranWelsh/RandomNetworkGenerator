cmake_minimum_required(VERSION 3.14...3.18)

set(VERSION_MAJOR 0)
set(VERSION_MINOR 0)
set(VERSION_MICRO 1)
SET(VERSION "${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_MICRO}")

project(RandomNetworkGenerator VERSION ${VERSION}
        DESCRIPTION "Random SBML network generator"
        LANGUAGES CXX
        )

########################################################################
#   Includes
#

# guard against in-source builds and bad build-type strings
include(cmake/safeguards.cmake)

# get git hash
include(cmake/git_revision.cmake)

# For incorporating dependencies into the main project
include(FetchContent)

##################################################################################
#   Project wide setup
#

# use C++ 17
set(CMAKE_CXX_STANDARD 17)
if (MSVC)
    # MSVC doesn't respect CMAKE_CXX_STANDARD
    set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /std:c++17")
endif (MSVC)


# detect operating system and host processor
message(STATUS "We are on a ${CMAKE_SYSTEM_NAME} system")
message(STATUS "The host processor is ${CMAKE_HOST_SYSTEM_PROCESSOR}")

# place binaries and libraries according to GNU standards
include(GNUInstallDirs)
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_LIBDIR})
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/${CMAKE_INSTALL_BINDIR})

# ensure -fpic on systems that support it
set(CMAKE_POSITION_INDEPENDENT_CODE ON)

# set CMAKE_PREFIX_PATH. Users can and should override this variable
set(CMAKE_PREFIX_PATH "D:/CppLibraries" CACHE PATH "Top level directory containing installed dependencies")

# Set module path
list(APPEND CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

# set all visibility to hidden by default.
set(CMAKE_CXX_VISIBILITY_PRESET hidden)
set(CMAKE_VISIBILITY_INLINES_HIDDEN YES)

# options
option(BUILD_TESTS "Build the tests when set to ON" ON)
option(SIM_FEATURES "When TRUE, RNG can run simulations and check for things like steady state" OFF)

#####################################################################
#   Deal with Dependencies
#

# dependency path
set(RNG_DEPENDENCIES_DIR "${CMAKE_CURRENT_SOURCE_DIR}/dependencies")
add_subdirectory(dependencies)


#########################################################################
#   Add project binaries
#

add_subdirectory(src)


###########################################################3
#   Tests
#

# this comparison ensures tests do not get built if this project gets embedded
# into another project
if (CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    add_subdirectory(test)
endif ()


#################################################################
#   Build docs
#

if (${BUILD_DOCS})
    add_subdirectory(docs)
endif ()

################################################################
#   Packaging
#


# this comparison ensures packaging do not get built if this project gets embedded
# into another project
if (CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    add_subdirectory(packaging)
endif ()



















