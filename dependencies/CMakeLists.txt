######################################################################
#   googletest
#

if (${BUILD_TESTS})
    add_subdirectory(googletest)
endif ()


if (${SIM_FEATURES})
    # then we need to locate and use roadrunner.
endif()

#FetchContent_Declare(googletest
#        GIT_REPOSITORY https://github.com/google/googletest.git
#        GIT_TAG 703bd9caab50b139428cea1aaff9974ebee5742e # release-1.10.0
#        )
#
#FetchContent_MakeAvailable(googletest)













