######################################################################
#   googletest
#
#   Used for all things testing. Will only be built if
#   the ${BUILD_TESTS} option is set to ON
#

if (${BUILD_TESTS})
    add_subdirectory(googletest)
endif ()

##########################################################################
#   roadrunner
#
#   Enables simulating modules after construction to check
#   for properties like "has steady state". Only a placeholder for now
#

if (${SIM_FEATURES})
    # then we need to locate and use roadrunner.
endif ()


##########################################################################
#   yaml-cpp is a git submodule
#
#   used for reading yaml configuration files
#

add_subdirectory(yaml-cpp)

##########################################################################
#   pystring is a git submodule
#
#   provides Python like string object out of the box
#

add_subdirectory(pystring)

######################################################################
#   libsbml
#
#   We do not include libsbml as a submodule because it is a bit of a pain
#   to get working this way. Instead it is the users responsibility to
#   ensure libsbml is installed and findable by the system you are on.
#   If libsbml is installed in a non-standard location, this means adding
#   the libsbml install prefix to PATH. On linux/mac this should be done in ~.bashrc whilst
#   on windows: https://www.architectryan.com/2018/03/17/add-to-the-path-on-windows-10/
#
#   Additionally, the libsbml "comp" extension package is needed. The code below
#   checks for the existence of the comp package and fails is it is not present.
#   The comp package can be downloaded from "https://sourceforge.net/projects/sbml/files/libsbml/5.18.0/stable/".
#   Decompress the contents and copy into the libsbml source tree before building and installing.
#
#   Libsbml must be compiled with the WITH_CPP_NAMESPACE option set to OFF
#

find_path(LIBSBML_INSTALL_PREFIX
        NAMES include/sbml/SBMLTypes.h
        HINTS ~/CppLibraries/libsbml-without-namespace
        )

if (NOT EXISTS "${LIBSBML_INSTALL_PREFIX}")
    message(FATAL_ERROR "Cannot locate LIBSBML_INSTALL_PREFIX. Please give the root of the libsbml install directory to LIBSBML_INSTALL_PREFIX")
endif ()
find_file(_COMP_PACKAGE_CHECK
        NAMES CompExtension.h
        PATHS ${LIBSBML_INSTALL_PREFIX}/include/sbml/packages/comp/extension
        )

if (NOT _COMP_PACKAGE_CHECK)
    message(FATAL_ERROR "The comp extension for libsbml is required but not detected. Please ensure it is available. You \
do this by downloading the libsbml comp package from https://sourceforge.net/projects/sbml/files/libsbml/5.18.0/stable/ \
and copying its contents into the top level of your libsbml source tree. Then rerun libsbml cmake ensuring you \
pass -DENABLE_COMP=ON.")
endif ()

# Remove disposable variable
unset(_COMP_PACKAGE_CHECK)

# add the directory containing libsbml-config.cmake to prefix path
list(APPEND CMAKE_PREFIX_PATH "${LIBSBML_INSTALL_PREFIX}/lib/cmake")

# sbml package-config is libsbml on windows and sbml on unix (and at a guess, probably mac as well)
set_cross_platform(
        LIBSBML_PACKAGE
        libsbml # windows
        sbml # linux
        sbml # mac
)
find_package(${LIBSBML_PACKAGE} CONFIG REQUIRED)

########################################################################
#   Antimony
#
#   For encoding models in the antimony language and converting
#   them into sbml. Depends on libsbml. Antimony is a git submodule
#

add_subdirectory(antimony)















