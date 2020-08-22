######################################################################
#   googletest
#

if (${BUILD_TESTS})
    add_subdirectory(googletest)
endif ()


if (${SIM_FEATURES})
    # then we need to locate and use roadrunner.
endif()

# yaml-cpp use for reading yaml configuration files
add_subdirectory(yaml-cpp)

# pystring: for nice python like std::string
add_subdirectory(pystring)














